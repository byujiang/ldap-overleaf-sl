FROM local/ldap-overleaf-base:3.0.1
# FROM sharelatex/sharelatex:latest
# latest might not be tested 
# e.g. the AuthenticationManager.js script had to be adapted after versions 2.3.1 
LABEL maintainer="Simon Haller-Seeber"
LABEL modifier="Yujiang Bi"
LABEL version="0.3"

# passed from .env (via make)
ARG collab_text
ARG login_text
ARG admin_is_sysadmin

# set workdir (might solve issue #2 - see https://stackoverflow.com/questions/57534295/)
WORKDIR /var/www/sharelatex/web

# overwrite some files
COPY sharelatex/AuthenticationManager.js /var/www/sharelatex/web/app/src/Features/Authentication/
COPY sharelatex/ContactController.js 	/var/www/sharelatex/web/app/src/Features/Contacts/

# Collaboration settings display (share project placeholder) | edit line 146
# share.pug file was removed in later versions

# extend pdflatex with option shell-esacpe ( fix for closed overleaf/overleaf/issues/217 and overleaf/docker-image/issues/45 )
# do this in different ways for different sharelatex versions
RUN sed -iE "s%-synctex=1\",%-synctex=1\", \"-shell-escape\",%g" /var/www/sharelatex/clsi/app/js/LatexRunner.js && \
    sed -iE "s%'-synctex=1',%'-synctex=1', '-shell-escape',%g" /var/www/sharelatex/clsi/app/js/LatexRunner.js

# Too much changes to do inline (>10 Lines).
COPY sharelatex/settings.pug 		/var/www/sharelatex/web/app/views/user/
COPY sharelatex/navbar.pug 		/var/www/sharelatex/web/app/views/layout/
# Login
COPY sharelatex/login.pug               /var/www/sharelatex/web/app/views/user/

# Non LDAP User Registration for Admins
#COPY sharelatex/admin-index.pug 	/var/www/sharelatex/web/app/views/admin/index.pug
RUN if [ "${admin_is_sysadmin}" = "true" ] ; then cp /tmp/admin-sysadmin.pug   /var/www/sharelatex/web/app/views/admin/index.pug ; else rm /tmp/admin-sysadmin.pug ; fi

# LOGO
COPY sharelatex/logo.png /var/www/sharelatex/web/public/

### To remove comments entirly (bug https://github.com/overleaf/overleaf/issues/678)
RUN rm /var/www/sharelatex/web/app/views/admin/register.pug && \
    rm /var/www/sharelatex/web/app/views/project/editor/review-panel.pug && \
    touch /var/www/sharelatex/web/app/views/project/editor/review-panel.pug

### Nginx and Certificates
# enable https via letsencrypt
#RUN  rm /etc/nginx/sites-enabled/sharelatex.conf

COPY nginx/options-ssl-nginx.conf /etc/nginx/
COPY nginx/ssl-dhparams.pem /etc/nginx/
