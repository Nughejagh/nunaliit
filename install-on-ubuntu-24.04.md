

$ sudo apt-get update

$ sudo apt upgrade
 
$ sudo apt install -y openjdk-8-jre-headless

$ sudo apt update

$ sudo apt install apt-transport-https curl gnupg

$ curl https://couchdb.apache.org/repo/keys.asc | /usr/bin/gpg --dearmor | sudo /usr/bin/tee /usr/share/keyrings/couchdb-archive-keyring.gpg >/dev/null 2>&1

$ echo "deb [signed-by=/usr/share/keyrings/couchdb-archive-keyring.gpg] https://apache.jfrog.io/artifactory/couchdb-deb/ bionic main" | sudo tee /etc/apt/sources.list.d/couchdb.list >/dev/null

$ sudo apt update

Try installing CouchDB:

$ sudo apt install -y couchdb=2.3.1~bionic

gives the error:

The following packages have unmet dependencies:
 couchdb : Depends: couch-libmozjs185-1.0 but it is not going to be installed
           Depends: libicu60 (>= 60.1-1~) but it is not installable
           Depends: libssl1.0.0 (>= 1.0.1) but it is not installable
           Depends: libtinfo5 (>= 6) but it is not installable
           Recommends: python3-progressbar but it is not going to be installed
E: Unable to correct problems, you have held broken packages.

We follow additional steps:

$ wget http://security.ubuntu.com/ubuntu/pool/main/i/icu/libicu60_60.2-3ubuntu3_amd64.deb

$ sudo dpkg -i libicu60_60.2-3ubuntu3_amd64.deb libssl1.0.0_1.0.2n-1ubuntu5.10_amd64.deb

$ echo "deb [trusted=yes] http://archive.ubuntu.com/ubuntu/ bionic main universe" | sudo tee -a /etc/apt/sources.list

$ sudo gedit /etc/apt/sources.list

Comment out

deb http://archive.ubuntu.com/ubuntu/ bionic main universe

$ sudo apt clean

$ sudo apt update

$ sudo apt install libssl1.0.0

Then we re-try installing CouchDB:

$ sudo apt install -y couchdb=2.3.1~bionic



Follow the screen to configure CouchDB:


    Choose "standalone"

    Enter bind address of "0.0.0.0"

    Choose a strong admin password. Nunaliit will ask for it during atlas creation.

$ sudo apt-mark hold couchdb

$ sudo vim /opt/couchdb/etc/local.ini

to add

max_http_request_size = 4294967296

to the [httpd] section.

$ sudo service couchdb restart

$ sudo service couchdb enable

couchdb URL:
http://127.0.0.1:5984/

$ sudo apt install -y imagemagick webp

$ sudo apt install -y ffmpeg ubuntu-restricted-extras


sudo vim /etc/magic

and append the following lines to it:
````
#Nunaliit Magic BEGIN
4       string          ftyp            ISO Media
>8      string          MSNV            \b, MPEG v4 system, version 2
!:mime  video/mp4
>8      string          isom            \b, MP4 Base Media v1
!:mime  video/mp4
>8      string/b        qt              video/quicktime
!:mime  video/quicktime
>8      string          3gp4             \b, Apple iTunes ALAC/AAC-LC (.M4A) Audio
!:mime  audio/x-m4a
#MP3 with ID3 tag
0       string      ID3     audio/mpeg
!:mime  audio/mpeg
#Nunaliit Magic END
````


$ sudo vim /etc/ImageMagick-6/policy.xml

add the line
````
<policy domain="coder" rights="read" pattern="PDF" />
````

Download https://github.com/GCRC/nunaliit/releases/download/2.2.9-SNAPSHOT/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040.zip and extract.

$ cd nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin

$ ./nunaliit create

Enter location where atlas should be created: nughejagh
Created atlas directory at: /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh
Copying templates from: /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/templates
Enter the name of the atlas [nughejagh]:
Enter the URL to CouchDB [http://127.0.0.1:5984/]:
Enter the name of the main database where atlas resides [nughejagh]:      
Do you wish to manually verify each document submission?(Y/N) [N]:
Enter the name of the admin user for CouchDB [admin]:
Enter the password for the admin user []: SAME-PASSWORD-AS-COUCHDB
Enter the port where the atlas is served [8080]:
Enter a Google Map API key (empty if not using) []:



$ ../nunaliit run

2024-12-04 22:15:05,419[INFO ]: Logging initialized @757ms to org.eclipse.jetty.util.log.Slf4jLog
2024-12-04 22:15:05,709[INFO ]: jetty-9.4.46.v20220331; built: 2022-03-31T16:38:08.030Z; git: bc17a0369a11ecf40bb92c839b9ef0a8ac50ea18; jvm 1.8.0_432-8u432-ga~us1-0ubuntu2~24.04-ga
2024-12-04 22:15:05,769[INFO ]: DefaultSessionIdManager workerName=node0
2024-12-04 22:15:05,769[INFO ]: No SessionScavenger set, using defaults
2024-12-04 22:15:05,779[INFO ]: node0 Scavenging every 660000ms
2024-12-04 22:15:06,502[INFO ]: Initializing Couch Configuration
2024-12-04 22:15:06,504[INFO ]: RNG: NativePRNG
2024-12-04 22:15:06,508[INFO ]: Document database configured: http://127.0.0.1:5984/nughejagh/
2024-12-04 22:15:06,509[INFO ]: User database configured: http://127.0.0.1:5984/_users/
2024-12-04 22:15:06,588[ERROR]: Error while updating server design document
javax.servlet.ServletException: Unable to update server design document
    at ca.carleton.gcrc.couch.command.servlet.ConfigServlet.initServerDesignDocument(ConfigServlet.java:497)
    at ca.carleton.gcrc.couch.command.servlet.ConfigServlet.init(ConfigServlet.java:166)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
Caused by: java.lang.Exception: Unable to access current document
    at ca.carleton.gcrc.couch.app.DocumentUpdateProcess.update(DocumentUpdateProcess.java:157)
    at ca.carleton.gcrc.couch.app.DocumentUpdateProcess.update(DocumentUpdateProcess.java:90)
    at ca.carleton.gcrc.couch.command.servlet.ConfigServlet.initServerDesignDocument(ConfigServlet.java:495)
    ... 30 more
Caused by: ca.carleton.gcrc.couch.client.impl.CouchDbException: CouchDB Error (404) not_found/Database does not exist.
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.checkResponseForError(ConnectionUtils.java:496)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:92)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:111)
    at ca.carleton.gcrc.couch.client.impl.CouchDbImpl.documentExists(CouchDbImpl.java:132)
    at ca.carleton.gcrc.couch.app.DocumentUpdateProcess.update(DocumentUpdateProcess.java:131)
    ... 32 more
2024-12-04 22:15:06,593[WARN ]: unavailable
java.lang.Exception: Unable to access current document
    at ca.carleton.gcrc.couch.app.DocumentUpdateProcess.update(DocumentUpdateProcess.java:157)
    at ca.carleton.gcrc.couch.app.DocumentUpdateProcess.update(DocumentUpdateProcess.java:90)
    at ca.carleton.gcrc.couch.command.servlet.ConfigServlet.initServerDesignDocument(ConfigServlet.java:495)
    at ca.carleton.gcrc.couch.command.servlet.ConfigServlet.init(ConfigServlet.java:166)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
Caused by: ca.carleton.gcrc.couch.client.impl.CouchDbException: CouchDB Error (404) not_found/Database does not exist.
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.checkResponseForError(ConnectionUtils.java:496)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:92)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:111)
    at ca.carleton.gcrc.couch.client.impl.CouchDbImpl.documentExists(CouchDbImpl.java:132)
    at ca.carleton.gcrc.couch.app.DocumentUpdateProcess.update(DocumentUpdateProcess.java:131)
    ... 32 more
2024-12-04 22:15:06,596[WARN ]: unavailable
javax.servlet.ServletException: Can not find configuration object
    at ca.carleton.gcrc.couch.date.DateServlet.init(DateServlet.java:43)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:15:06,597[WARN ]: unavailable
javax.servlet.ServletException: Atlas name is not specified (ExportServlet_AtlasName)
    at ca.carleton.gcrc.couch.export.ExportServlet.init(ExportServlet.java:75)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:15:06,598[INFO ]: Initialization started
2024-12-04 22:15:06,598[ERROR]: Document database is not specified: IndexServlet_DocumentDatabase
2024-12-04 22:15:06,599[WARN ]: unavailable
javax.servlet.ServletException: Document database is not specified (IndexServlet_DocumentDatabase)
    at ca.carleton.gcrc.couch.metadata.IndexServlet.init(IndexServlet.java:69)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:15:06,600[INFO ]: Initialization started
2024-12-04 22:15:06,600[ERROR]: Robots DB change listener is not specified: RobotsServlet_RobotsDbChangeListener
2024-12-04 22:15:06,600[WARN ]: unavailable
javax.servlet.ServletException: Robots DB change listener is not specified (RobotsServlet_RobotsDbChangeListener)
    at ca.carleton.gcrc.couch.metadata.RobotsServlet.init(RobotsServlet.java:41)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:15:06,601[INFO ]: Initialization started
2024-12-04 22:15:06,601[ERROR]: SitemapBuilder is not specified (SitemapServlet_SitemapBuilder)
2024-12-04 22:15:06,602[WARN ]: unavailable
javax.servlet.ServletException: SitemapBuilder is not specified (SitemapServlet_SitemapBuilder)
    at ca.carleton.gcrc.couch.metadata.SitemapServlet.init(SitemapServlet.java:45)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:15:06,602[WARN ]: unavailable
javax.servlet.ServletException: Can not find configuration object
    at ca.carleton.gcrc.couch.simplifiedGeometry.SimplifiedGeometryServlet.init(SimplifiedGeometryServlet.java:43)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:15:06,603[INFO ]: SubmissionServlet servlet initialization - start
2024-12-04 22:15:06,603[WARN ]: unavailable
javax.servlet.ServletException: Atlas name is not specified (SubmissionServlet_AtlasName)
    at ca.carleton.gcrc.couch.submission.SubmissionServlet.init(SubmissionServlet.java:68)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:15:06,604[INFO ]: UserServlet servlet initialization - start
2024-12-04 22:15:06,604[WARN ]: unavailable
javax.servlet.ServletException: Atlas name is not specified (UserServlet_AtlasName)
    at ca.carleton.gcrc.couch.user.UserServlet.init(UserServlet.java:73)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:15:06,605[WARN ]: unavailable
javax.servlet.ServletException: Can not find configuration object
    at ca.carleton.gcrc.mail.MailServlet.init(MailServlet.java:37)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:15:06,608[ERROR]: Property file location can not be determined
2024-12-04 22:15:06,608[INFO ]: Repository directory is /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/.
2024-12-04 22:15:06,609[INFO ]: OnUploadedListener: OnUploadedListenerWrapper
2024-12-04 22:15:06,611[INFO ]: node0 Stopped scavenging
2024-12-04 22:15:36,616[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-13,5,main]
2024-12-04 22:15:36,617[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-14,5,main]
2024-12-04 22:15:36,618[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-15,5,main]
2024-12-04 22:15:36,618[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-16,5,main]
2024-12-04 22:15:36,619[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-12,5,main]
2024-12-04 22:15:36,619[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-11,5,main]
Error: Multiple exceptions
Caused by: Unable to update server design document
Caused by: Unable to access current document
Caused by: CouchDB Error (404) not_found/Database does not exist.
pradeeban@llovizna:~/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh$


Use CouchDB frontend http://127.0.0.1:5984/_utils/#login

Create DB "nughejagh"


$ ../nunaliit run
2024-12-04 22:43:33,219[INFO ]: Logging initialized @314ms to org.eclipse.jetty.util.log.Slf4jLog
2024-12-04 22:43:33,390[INFO ]: jetty-9.4.46.v20220331; built: 2022-03-31T16:38:08.030Z; git: bc17a0369a11ecf40bb92c839b9ef0a8ac50ea18; jvm 1.8.0_432-8u432-ga~us1-0ubuntu2~24.04-ga
2024-12-04 22:43:33,421[INFO ]: DefaultSessionIdManager workerName=node0
2024-12-04 22:43:33,421[INFO ]: No SessionScavenger set, using defaults
2024-12-04 22:43:33,425[INFO ]: node0 Scavenging every 660000ms
2024-12-04 22:43:33,652[INFO ]: Initializing Couch Configuration
2024-12-04 22:43:33,653[INFO ]: RNG: NativePRNG
2024-12-04 22:43:33,659[INFO ]: Document database configured: http://127.0.0.1:5984/nughejagh/
2024-12-04 22:43:33,660[INFO ]: User database configured: http://127.0.0.1:5984/_users/
2024-12-04 22:43:34,198[INFO ]: User design document. Disk: 3 Db: 0 Update Required: true
2024-12-04 22:43:34,251[INFO ]: Reading properties from /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./config/multimedia.properties
2024-12-04 22:43:34,276[INFO ]: Reading properties from /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./config/sensitive.properties
2024-12-04 22:43:34,277[INFO ]: Reading properties from /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./config/mail.properties
2024-12-04 22:43:34,292[INFO ]: sendUploadMailNotification: false
2024-12-04 22:43:34,299[INFO ]: sendUploadMailNotification: false
2024-12-04 22:43:34,300[INFO ]: Reading properties from /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./config/upload.properties
2024-12-04 22:43:34,301[INFO ]: Repository directory is /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./media
2024-12-04 22:43:34,309[INFO ]: uploadOriginalFiles: true
2024-12-04 22:43:34,313[INFO ]: uploadOriginalFiles: true
2024-12-04 22:43:34,327[INFO ]: Start database change monitor thread
2024-12-04 22:43:34,331[INFO ]: Submission database is not enabled
2024-12-04 22:43:34,331[INFO ]: Start upload worker thread
2024-12-04 22:43:34,333[INFO ]: Vetter daily notifications set to start at: 2024-12-05 00:00:00
2024-12-04 22:43:34,395[INFO ]: Starting DB change listener thread
2024-12-04 22:43:34,402[ERROR]: Problem fetching docId _design/site from database
ca.carleton.gcrc.couch.client.impl.CouchDbException: CouchDB Error (404) not_found/missing
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.checkResponseForError(ConnectionUtils.java:496)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:92)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:111)
    at ca.carleton.gcrc.couch.client.impl.CouchDbImpl.getDocument(CouchDbImpl.java:167)
    at ca.carleton.gcrc.couch.client.impl.listener.AttachmentChangeListener.updateDocument(AttachmentChangeListener.java:131)
    at ca.carleton.gcrc.couch.client.impl.listener.AttachmentChangeListener.<init>(AttachmentChangeListener.java:65)
    at ca.carleton.gcrc.couch.client.impl.listener.HtmlAttachmentChangeListener.<init>(HtmlAttachmentChangeListener.java:25)
    at ca.carleton.gcrc.couch.command.servlet.ConfigServlet.initIndex(ConfigServlet.java:916)
    at ca.carleton.gcrc.couch.command.servlet.ConfigServlet.init(ConfigServlet.java:276)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:43:34,405[INFO ]: Starting DB change listener thread
2024-12-04 22:43:34,407[INFO ]: Starting DB change listener thread
2024-12-04 22:43:34,409[ERROR]: Problem querying atlas design document view 'modules'
ca.carleton.gcrc.couch.client.impl.CouchDbException: CouchDB Error (404) not_found/missing
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.checkResponseForError(ConnectionUtils.java:496)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:92)
    at ca.carleton.gcrc.couch.client.impl.CouchDesignDocumentImpl.performQuery(CouchDesignDocumentImpl.java:117)
    at ca.carleton.gcrc.couch.client.impl.CouchDesignDocumentImpl.performQuery(CouchDesignDocumentImpl.java:43)
    at ca.carleton.gcrc.couch.client.impl.listener.ModuleMetadataChangeListener.findAllModuleIds(ModuleMetadataChangeListener.java:132)
    at ca.carleton.gcrc.couch.client.impl.listener.ModuleMetadataChangeListener.performStartupTasks(ModuleMetadataChangeListener.java:86)
    at ca.carleton.gcrc.couch.client.impl.listener.AbstractCouchDbChangeListener.run(AbstractCouchDbChangeListener.java:61)
2024-12-04 22:43:34,412[ERROR]: Problem fetching docId _design/site from database
ca.carleton.gcrc.couch.client.impl.CouchDbException: CouchDB Error (404) not_found/missing
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.checkResponseForError(ConnectionUtils.java:496)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:92)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:111)
    at ca.carleton.gcrc.couch.client.impl.CouchDbImpl.getDocument(CouchDbImpl.java:167)
    at ca.carleton.gcrc.couch.client.impl.listener.AttachmentChangeListener.updateDocument(AttachmentChangeListener.java:131)
    at ca.carleton.gcrc.couch.client.impl.listener.AttachmentChangeListener.<init>(AttachmentChangeListener.java:65)
    at ca.carleton.gcrc.couch.client.impl.listener.TextAttachmentChangeListener.<init>(TextAttachmentChangeListener.java:24)
    at ca.carleton.gcrc.couch.command.servlet.ConfigServlet.initRobots(ConfigServlet.java:935)
    at ca.carleton.gcrc.couch.command.servlet.ConfigServlet.init(ConfigServlet.java:284)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
2024-12-04 22:43:34,413[INFO ]: Completed Couch Configuration
2024-12-04 22:43:34,413[INFO ]: Starting DB change listener thread
2024-12-04 22:43:34,485[INFO ]: Unable to retrieve date cluster information
java.lang.Exception: Unable to retrieve date cluster information
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.loadTree(CouchTreeOperations.java:129)
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:23)
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:16)
    at ca.carleton.gcrc.couch.date.impl.DateSourceCouchWithCluster.<init>(DateSourceCouchWithCluster.java:30)
    at ca.carleton.gcrc.couch.date.DateServlet.init(DateServlet.java:50)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
Caused by: ca.carleton.gcrc.couch.client.impl.CouchDbException: CouchDB Error (404) not_found/missing
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.checkResponseForError(ConnectionUtils.java:496)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:92)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:111)
    at ca.carleton.gcrc.couch.client.impl.CouchDbImpl.getDocument(CouchDbImpl.java:167)
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.loadTree(CouchTreeOperations.java:126)
    ... 33 more
2024-12-04 22:43:34,489[ERROR]: Unable to recover date cluster tree
java.lang.Exception: Unable to retrieve date interval elements
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.recoverTree(CouchTreeOperations.java:152)
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:31)
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:16)
    at ca.carleton.gcrc.couch.date.impl.DateSourceCouchWithCluster.<init>(DateSourceCouchWithCluster.java:30)
    at ca.carleton.gcrc.couch.date.DateServlet.init(DateServlet.java:50)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
Caused by: ca.carleton.gcrc.couch.client.impl.CouchDbException: CouchDB Error (404) not_found/missing
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.checkResponseForError(ConnectionUtils.java:496)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:92)
    at ca.carleton.gcrc.couch.client.impl.CouchDesignDocumentImpl.performQuery(CouchDesignDocumentImpl.java:117)
    at ca.carleton.gcrc.couch.client.impl.CouchDesignDocumentImpl.performQuery(CouchDesignDocumentImpl.java:43)
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.getAllElements(CouchTreeOperations.java:38)
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.recoverTree(CouchTreeOperations.java:150)
    ... 33 more
2024-12-04 22:43:34,490[ERROR]: Unable to create a date cluster tree
java.lang.Exception: Unable to recover date cluster tree
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:37)
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:16)
    at ca.carleton.gcrc.couch.date.impl.DateSourceCouchWithCluster.<init>(DateSourceCouchWithCluster.java:30)
    at ca.carleton.gcrc.couch.date.DateServlet.init(DateServlet.java:50)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
Caused by: java.lang.Exception: Unable to retrieve date interval elements
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.recoverTree(CouchTreeOperations.java:152)
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:31)
    ... 32 more
Caused by: ca.carleton.gcrc.couch.client.impl.CouchDbException: CouchDB Error (404) not_found/missing
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.checkResponseForError(ConnectionUtils.java:496)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:92)
    at ca.carleton.gcrc.couch.client.impl.CouchDesignDocumentImpl.performQuery(CouchDesignDocumentImpl.java:117)
    at ca.carleton.gcrc.couch.client.impl.CouchDesignDocumentImpl.performQuery(CouchDesignDocumentImpl.java:43)
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.getAllElements(CouchTreeOperations.java:38)
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.recoverTree(CouchTreeOperations.java:150)
    ... 33 more
2024-12-04 22:43:34,491[WARN ]: unavailable
java.lang.Exception: Unable to create a date cluster tree
    at ca.carleton.gcrc.couch.date.impl.DateSourceCouchWithCluster.<init>(DateSourceCouchWithCluster.java:33)
    at ca.carleton.gcrc.couch.date.DateServlet.init(DateServlet.java:50)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
Caused by: java.lang.Exception: Unable to recover date cluster tree
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:37)
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:16)
    at ca.carleton.gcrc.couch.date.impl.DateSourceCouchWithCluster.<init>(DateSourceCouchWithCluster.java:30)
    ... 30 more
Caused by: java.lang.Exception: Unable to retrieve date interval elements
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.recoverTree(CouchTreeOperations.java:152)
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:31)
    ... 32 more
Caused by: ca.carleton.gcrc.couch.client.impl.CouchDbException: CouchDB Error (404) not_found/missing
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.checkResponseForError(ConnectionUtils.java:496)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:92)
    at ca.carleton.gcrc.couch.client.impl.CouchDesignDocumentImpl.performQuery(CouchDesignDocumentImpl.java:117)
    at ca.carleton.gcrc.couch.client.impl.CouchDesignDocumentImpl.performQuery(CouchDesignDocumentImpl.java:43)
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.getAllElements(CouchTreeOperations.java:38)
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.recoverTree(CouchTreeOperations.java:150)
    ... 33 more
2024-12-04 22:43:34,494[INFO ]: Initialization started
2024-12-04 22:43:34,494[INFO ]: Initialization finished
2024-12-04 22:43:34,495[INFO ]: Initialization started
2024-12-04 22:43:34,495[INFO ]: Initialization finished
2024-12-04 22:43:34,495[INFO ]: Initialization started
2024-12-04 22:43:34,495[INFO ]: Initialization finished
2024-12-04 22:43:34,497[INFO ]: SubmissionServlet servlet initialization - start
2024-12-04 22:43:34,498[INFO ]: SubmissionServlet servlet initialization - completed
2024-12-04 22:43:34,499[INFO ]: UserServlet servlet initialization - start
2024-12-04 22:43:34,499[WARN ]: Cannot get navigation info from document 'atlas'
2024-12-04 22:43:34,563[INFO ]: UserServlet servlet initialization - completed
2024-12-04 22:43:34,563[INFO ]: Start agreement worker thread
2024-12-04 22:43:34,567[INFO ]: Upload Property atlas.name = nughejagh
2024-12-04 22:43:34,567[INFO ]: Upload Property upload.repository.dir = /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./media
2024-12-04 22:43:34,567[INFO ]: OnUploadedListener: UploadListener
2024-12-04 22:43:34,568[INFO ]: node0 Stopped scavenging
2024-12-04 22:43:34,569[ERROR]: User agreement document not found in database
2024-12-04 22:44:04,576[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-13,5,main]
2024-12-04 22:44:04,595[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-14,5,main]
2024-12-04 22:44:04,596[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-15,5,main]
2024-12-04 22:44:04,598[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-16,5,main]
2024-12-04 22:44:04,599[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-12,5,main]
2024-12-04 22:44:04,600[WARN ]: QueuedThreadPool[qtp2036958521]@79698539{STOPPING,8<=0<=200,i=2,r=-1,q=0}[NO_TRY] Couldn't stop Thread[qtp2036958521-11,5,main]
Error: Unable to create date source
Caused by: Unable to create a date cluster tree
Caused by: Unable to recover date cluster tree
Caused by: Unable to retrieve date interval elements
Caused by: CouchDB Error (404) not_found/missing
pradeeban@llovizna:~/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh$

Nunaliit on Ubuntu 24.04

$ ../nunaliit update
pradeeban@llovizna:~/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh

$ ../nunaliit run
2024-12-04 22:59:00,744[INFO ]: Logging initialized @344ms to org.eclipse.jetty.util.log.Slf4jLog
2024-12-04 22:59:00,950[INFO ]: jetty-9.4.46.v20220331; built: 2022-03-31T16:38:08.030Z; git: bc17a0369a11ecf40bb92c839b9ef0a8ac50ea18; jvm 1.8.0_432-8u432-ga~us1-0ubuntu2~24.04-ga
2024-12-04 22:59:00,979[INFO ]: DefaultSessionIdManager workerName=node0
2024-12-04 22:59:00,980[INFO ]: No SessionScavenger set, using defaults
2024-12-04 22:59:00,984[INFO ]: node0 Scavenging every 600000ms
2024-12-04 22:59:01,199[INFO ]: Initializing Couch Configuration
2024-12-04 22:59:01,201[INFO ]: RNG: NativePRNG
2024-12-04 22:59:01,205[INFO ]: Document database configured: http://127.0.0.1:5984/nughejagh/
2024-12-04 22:59:01,207[INFO ]: User database configured: http://127.0.0.1:5984/_users/
2024-12-04 22:59:01,281[INFO ]: User design document. Disk: 3 Db: 3 Update Required: true
2024-12-04 22:59:01,320[INFO ]: Reading properties from /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./config/multimedia.properties
2024-12-04 22:59:01,336[INFO ]: Reading properties from /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./config/sensitive.properties
2024-12-04 22:59:01,336[INFO ]: Reading properties from /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./config/mail.properties
2024-12-04 22:59:01,353[INFO ]: sendUploadMailNotification: false
2024-12-04 22:59:01,365[INFO ]: sendUploadMailNotification: false
2024-12-04 22:59:01,366[INFO ]: Reading properties from /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./config/upload.properties
2024-12-04 22:59:01,367[INFO ]: Repository directory is /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./media
2024-12-04 22:59:01,372[INFO ]: uploadOriginalFiles: true
2024-12-04 22:59:01,377[INFO ]: uploadOriginalFiles: true
2024-12-04 22:59:01,406[INFO ]: Start database change monitor thread
2024-12-04 22:59:01,412[INFO ]: Submission database is not enabled
2024-12-04 22:59:01,412[INFO ]: Start upload worker thread
2024-12-04 22:59:01,415[INFO ]: Vetter daily notifications set to start at: 2024-12-05 00:00:00
2024-12-04 22:59:01,430[INFO ]: Starting DB change listener thread
2024-12-04 22:59:01,605[INFO ]: Starting DB change listener thread
2024-12-04 22:59:01,607[INFO ]: Starting DB change listener thread
2024-12-04 22:59:01,627[INFO ]: Completed Couch Configuration
2024-12-04 22:59:01,627[INFO ]: Starting DB change listener thread
2024-12-04 22:59:01,649[INFO ]: Unable to retrieve date cluster information
java.lang.Exception: Unable to retrieve date cluster information
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.loadTree(CouchTreeOperations.java:129)
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:23)
    at ca.carleton.gcrc.couch.date.cluster.CouchIntervalClusterTreeFactory.createClusterTree(CouchIntervalClusterTreeFactory.java:16)
    at ca.carleton.gcrc.couch.date.impl.DateSourceCouchWithCluster.<init>(DateSourceCouchWithCluster.java:30)
    at ca.carleton.gcrc.couch.date.DateServlet.init(DateServlet.java:50)
    at org.eclipse.jetty.servlet.ServletHolder.initServlet(ServletHolder.java:632)
    at org.eclipse.jetty.servlet.ServletHolder.initialize(ServletHolder.java:415)
    at org.eclipse.jetty.servlet.ServletHandler.lambda$initialize$0(ServletHandler.java:731)
    at java.util.stream.SortedOps$SizedRefSortingSink.end(SortedOps.java:357)
    at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:483)
    at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:472)
    at java.util.stream.StreamSpliterators$WrappingSpliterator.forEachRemaining(StreamSpliterators.java:313)
    at java.util.stream.Streams$ConcatSpliterator.forEachRemaining(Streams.java:743)
    at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
    at org.eclipse.jetty.servlet.ServletHandler.initialize(ServletHandler.java:755)
    at org.eclipse.jetty.servlet.ServletContextHandler.startContext(ServletContextHandler.java:379)
    at org.eclipse.jetty.server.handler.ContextHandler.doStart(ContextHandler.java:916)
    at org.eclipse.jetty.servlet.ServletContextHandler.doStart(ServletContextHandler.java:288)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.handler.gzip.GzipHandler.doStart(GzipHandler.java:426)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.start(ContainerLifeCycle.java:169)
    at org.eclipse.jetty.server.Server.start(Server.java:423)
    at org.eclipse.jetty.util.component.ContainerLifeCycle.doStart(ContainerLifeCycle.java:110)
    at org.eclipse.jetty.server.handler.AbstractHandler.doStart(AbstractHandler.java:97)
    at org.eclipse.jetty.server.Server.doStart(Server.java:387)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:73)
    at ca.carleton.gcrc.couch.command.CommandRun.runCommand(CommandRun.java:298)
    at ca.carleton.gcrc.couch.command.Main.performCommand(Main.java:241)
    at ca.carleton.gcrc.couch.command.Main.execute(Main.java:168)
    at ca.carleton.gcrc.couch.command.Main.main(Main.java:64)
Caused by: ca.carleton.gcrc.couch.client.impl.CouchDbException: CouchDB Error (404) not_found/missing
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.checkResponseForError(ConnectionUtils.java:496)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:92)
    at ca.carleton.gcrc.couch.client.impl.ConnectionUtils.getJsonResource(ConnectionUtils.java:111)
    at ca.carleton.gcrc.couch.client.impl.CouchDbImpl.getDocument(CouchDbImpl.java:167)
    at ca.carleton.gcrc.couch.date.cluster.CouchTreeOperations.loadTree(CouchTreeOperations.java:126)
    ... 33 more
2024-12-04 22:59:02,783[INFO ]: Recovered date cluster tree
2024-12-04 22:59:02,805[INFO ]: Start date worker thread
2024-12-04 22:59:02,806[INFO ]: Initialization started
2024-12-04 22:59:02,807[INFO ]: Initialization finished
2024-12-04 22:59:02,808[INFO ]: Initialization started
2024-12-04 22:59:02,808[INFO ]: Initialization finished
2024-12-04 22:59:02,808[INFO ]: Initialization started
2024-12-04 22:59:02,808[INFO ]: Initialization finished
2024-12-04 22:59:02,810[INFO ]: SubmissionServlet servlet initialization - start
2024-12-04 22:59:02,812[INFO ]: SubmissionServlet servlet initialization - completed
2024-12-04 22:59:02,813[INFO ]: UserServlet servlet initialization - start
2024-12-04 22:59:02,834[INFO ]: UserServlet servlet initialization - completed
2024-12-04 22:59:02,834[INFO ]: Start agreement worker thread
2024-12-04 22:59:02,850[INFO ]: Upload Property atlas.name = nughejagh
2024-12-04 22:59:02,850[INFO ]: Upload Property upload.repository.dir = /home/pradeeban/Documents/nunaliit_2.2.9-SNAPSHOT_2024-12-03_c313040/bin/nughejagh/./media
2024-12-04 22:59:02,850[INFO ]: OnUploadedListener: UploadListener
2024-12-04 22:59:02,858[INFO ]: Started o.e.j.s.ServletContextHandler@5aa9e4eb{/,null,AVAILABLE}
2024-12-04 22:59:02,896[INFO ]: New user agreement detected: 1-01f0095897c27eb79dbad08f073605a2
2024-12-04 22:59:02,896[INFO ]: User agreement is not enabled. Skip.
2024-12-04 22:59:02,935[INFO ]: Started ServerConnector@3ffc5af1{HTTP/1.1, (http/1.1)}{0.0.0.0:8080}
2024-12-04 22:59:02,936[INFO ]: Started @2538ms

Now, Nunaliit can be accessed from http://localhost:8080

