# build

	docker build -t="tetras-libre/subversion" .

# Prepare

Create the volumes

    mkdir -p /srv/subversion/data/repos
    mkdir -p /srv/subversion/conf

create a authz file `/srv/subversion/conf/svn-access` :

    [groups]
    admin = me

    [/]
    @me = rw
    * = r

and a htpasswd file `/srv/subversion/conf/svn-password` :

    htpasswd -c /srv/subversion/conf/svn-password me


# run

	docker run --name subversion -d -p 80:80 -v /srv/subversion/data:/var/svn -v /srv/subversion/conf:/etc/svn rkrx/subversion

# Create a repository

    svnadmin create /srv/subversion/data/repos/<my-repo>
    chown -R www-data:www-data /srv/subversion/data

# cleanup

	docker kill subversion
	docker rm subversion
	docker rmi rkrx/subversion
