---
layout: post
title: 'Authentication Challenge with multiple in-house certificate authorities in iOS with Swift'
tags: swift certificates ssl pinning ios cacert root-ca self-signed https handshake
comments: true
permalink: swift-authentication-challenge
share: true
---

In one of my recent projects there was a requirement for a certificate and public key pinning.
The API performs its operation from servers with certificates signed by an in-house certificate authority (self-signed too of course).

But there are mulitple CA, and clients servers can have different CA, so a hard-coding approach was not the best. 
So the idea is to validate the certificate to ensure that is was signed by any of the provided trusted CA.


In this example we are going to implement the protocol NSURLSessionDelegate specifically the function which handles server certificate validation

So lets see the code


``` swift

public func URLSession(session: NSURLSession, didReceiveChallenge challenge: NSURLAuthenticationChallenge, completionHandler: (NSURLSessionAuthChallengeDisposition, NSURLCredential?) -> Void) {
        
        if challenge.protectionSpace.authenticationMethod == NSURLAuthenticationMethodServerTrust {

            let trust = challenge.protectionSpace.serverTrust
                        
            let rootCaCerts = self.rootCACertificatesData.map() {
                (data) -> CFData? in
                return CFDataCreate(kCFAllocatorDefault, UnsafePointer(data.bytes), data.length)
                }.filter({$0 != nil}).map() { ref -> CFTypeRef in
                    return SecCertificateCreateWithData(kCFAllocatorDefault, ref!)!
            }
            
            let certArrayRef : CFArrayRef = CFBridgingRetain(rootCaCerts as NSArray) as! CFArrayRef
            
            SecTrustSetAnchorCertificates(trust!, certArrayRef)
            SecTrustSetAnchorCertificatesOnly(trust!, true) // if "true" then also allows certificates signed with one of the system available root certificates.
            
            var trustResult: SecTrustResultType = 0
            SecTrustEvaluate(trust!, &trustResult)
             
            if (Int(trustResult) == kSecTrustResultUnspecified ||
                Int(trustResult) == kSecTrustResultProceed) {
                    //Trust the server certificate cause its signed with one of the allowed in-house CA
                    let credential = NSURLCredential(forTrust: challenge.protectionSpace.serverTrust!)
                    challenge.sender!.useCredential(credential, forAuthenticationChallenge: challenge)
                    completionHandler(NSURLSessionAuthChallengeDisposition.UseCredential, credential)
            } else {
                print("Invalid server certificate.") //this also happens with expired certificates
                challenge.sender!.cancelAuthenticationChallenge(challenge)
                completionHandler(NSURLSessionAuthChallengeDisposition.CancelAuthenticationChallenge, nil)
            }

        } else {
            print("Unexpected authentication method");
            challenge.sender!.cancelAuthenticationChallenge(challenge)
            completionHandler(NSURLSessionAuthChallengeDisposition.CancelAuthenticationChallenge, nil)
        }

    }

```


So, trust or die xD

`rootCACertificatesData` its an NSData array with the CA certificates file content .... DER encoded certificates added to the project.

So insted of a list with files names, lets find all the "DER" files and load them into an array:

```swift

	let enumerator = NSFileManager.defaultManager().enumeratorAtPath(NSBundle.mainBundle().bundlePath)
    while let filePath = enumerator?.nextObject() as? String {
    	if NSURL(fileURLWithPath: filePath).pathExtension == "der" {
        	if let data = NSData(contentsOfURL:(NSBundle.mainBundle().bundleURL.URLByAppendingPathComponent(filePath))) {
            	self.rootCaFiles.append(data)
            }
        }
    }

```


