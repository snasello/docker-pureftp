
thanks to https://github.com/stilliard/docker-pure-ftpd and https://github.com/mazelab/docker-ftp

I combine the files from those repositories to make one with ftp  tls

Starting it 
------------------------------
`touch pureftpd.passwd`

`docker run -p 21:21 -d --name ftp -v $(pwd)/datas:/datas snasello/ftp`

Operating it
------------------------------

`docker exec -it ftp /bin/bash`

Example usage once inside
------------------------------

Create an ftp user: `e.g. bob with chroot access only to /datas/ftpusers/bob`
```bash
pure-pw useradd bob -u ftpuser -d /datas/ftpusers/bob -f /datas/pureftpd.passwd
pure-pw mkdb /datas/pureftpd.pdb -f /datas/pureftpd.passwd
```
*No restart should be needed.*

More info on usage here:

- http://download.pureftpd.org/pure-ftpd/doc/README.Virtual-Users
- http://www.debianhelp.co.uk/pureftp.htm


Credits
-------------
Thanks for the help on stackoverflow with this!
http://stackoverflow.com/questions/23930167/installing-pure-ftpd-in-docker-debian-wheezy-error-421


TLS
-------------

The container runs default in FTP over TLS only. You can overwrite the dummy certificate with -v myCert.pem:/etc/ssl/private/pure-ftpd.pem . You can also change the TLS behavior with the environment variable FTP_TLS.

Run the container with mounted storage, overwrite pure-ftp passwd config and use a custom certificate:
`
docker run -p 21:21 -d --name ftp -v $(pwd)/datas:/datas -v $(pwd)/myCert.pem:/etc/ssl/private/pure-ftpd.pem snasello/ftp
`


