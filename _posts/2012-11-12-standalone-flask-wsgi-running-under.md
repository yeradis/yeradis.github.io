---
layout: post
title: Standalone Flask WSGI running under Tornado , Twisted or Built-in server
date: 2012-11-12
tags: 
comments: true
permalink:
share: true
---

Make your Flask Microframework stuff "portable" and ready for "production" with Tornado or Twisted. 

And if necessary, you can turn on the development server.




Is true that "Micro" does not mean that your whole web application has to fit into a single Python file, but this sample goes in that way.




I writing this, because the built-in server provided by Flask is great but just for development, can be tuned, still not good enough, so, what about if you want your flask ready for "production", something like serving 1000 request/s ?




In this sample, you will find how to run your Flask WSGI under Tornado and Twisted, but in the embedded way.




Just to remember:




[Tornado][1] is an open source version of the scalable, non-blocking web server and tools that power FriendFeed.




[Twisted][2] is an event-driven networking engine that supports many common network protocols, including SMTP, POP3, IMAP, SSHv2, and DNS.




So, here you go:





    import optparse
    from flask import Flask
    app = Flask(__name__)

    @app.route('/')
    def hello():
        return 'Hello World!!! A Flask WSGI under Twisted/Tornado/Built-in development server...'


    __port = 701

    def getPort(value):
        return (__port, value)[value > 0]

    def tornado(option, opt_str, value, parser):
        print 'Tornado on port {port}...'.format(port=getPort(value))
        from tornado.wsgi import WSGIContainer
        from tornado.httpserver import HTTPServer
        from tornado.ioloop import IOLoop

        http_server = HTTPServer(WSGIContainer(app))
        http_server.listen(getPort(value))
        IOLoop.instance().start()

    def twisted(option, opt_str, value, parser):
        print 'Twisted on port {port}...'.format(port=getPort(value))
        from twisted.internet import reactor
        from twisted.web.server import Site
        from twisted.web.wsgi import WSGIResource

        resource = WSGIResource(reactor, reactor.getThreadPool(), app)
        site = Site(resource)

        reactor.listenTCP(getPort(value), site, interface="0.0.0.0")
        reactor.run()

    def builtin(option, opt_str, value, parser):
        print 'Built-in development server on port {port}...'.format(port=getPort(value))
        app.run(host="0.0.0.0",port=getPort(value),debug=True)


    def main():
        parser = optparse.OptionParser(usage="%prog [options]  or type %prog -h (--help)")
        parser.add_option('--tornado', help='Tornado non-blocking web server', action="callback", callback=tornado,type="int");
        parser.add_option('--twisted', help='Twisted event-driven web server', action="callback", callback=twisted, type="int");
        parser.add_option('--builtin', help='Built-in Flask web development server', action="callback", callback=builtin, type="int");
        (options, args) = parser.parse_args()
        parser.print_help()

    if __name__ == "__main__":
        main()

















"));

[1]: http://www.tornadoweb.org/
[2]: http://twistedmatrix.com/
