# OpenStack UI Plugin

## General

Integration with the Openstack interface is our second plugin alongside the oVirt/RHV virtualization family. Thanks to it, you can perform most of the basic operations without logging into the vProtect dashboard.

After installation \(which is described at the end of this article\) you will see a new tab "vProtect" in the OpenStack menu.

![](../../.gitbook/assets/integration-plugins-openstack-dashboard.jpg)

### Dashboard

The dashboard consists of several tabs that allow you to perform basic actions such as backup, restore or create a new schedule.

![](../../.gitbook/assets/integration-plugins-openstack-dashboard2.jpg)

### Backup

This tab shows all inventoried instances in your OpenStack environment.

![](../../.gitbook/assets/integration-plugins-openstack-backup.jpg)

Besides, you can also perform basic backup operations.

![](../../.gitbook/assets/integration-plugins-openstack-backup2.jpg)

### Restore

This tab displays all instances in the OpenStack environment that can be restored.

![](../../.gitbook/assets/integration-plugins-openstack-restore.jpg)

Restore window:

![](../../.gitbook/assets/integration-plugins-openstack-restore2.jpg)

### Schedule

As the name suggests, this tab allows you to create schedules, but not only.

![](../../.gitbook/assets/integration-plugins-openstack-schedule.jpg)

Thanks to it, we will also create the policies necessary for their operation.

![](../../.gitbook/assets/integration-plugins-openstack-schedule2.jpg)

### Tasks

Basic information about current tasks performed by vProtect.

![](../../.gitbook/assets/integration-plugins-openstack-tasks.jpg)

## OpenStack general integration setup

You can find the add-on in the [GitHub repository](https://github.com/Storware/openstack-horizon-ui-vprotect-extensions). Extract the provided archive on to your Horizon host and execute `python install.py VPROTECT_API_URL USER PASSWORD`

**Example:** `python install.py http://localhost:8080/api admin vPr0tect`.

**Note:** you need to restart your Horizon HTTP server after this

The above-mentioned script will copy the plug-in files to the following folders:

* `/usr/share/openstack-dashboard/openstack_dashboard/dashboards/vprotect` - plugin files
* `/usr/share/openstack-dashboard/openstack_dashboard/enabled` - file to enable plugin

In order to **uninstall** it, remove the `vprotect` subfolder and `enabled/_50_vprotect.py` file and restart your Horizon HTTP server.

### Integrate vProtect dashboard plugin to OpenStack (LXC)

**Requirements:**

* git, python3-yaml packages
* internet connection

1. Check name of the horizon container:

    ```text
    lxc-ls -f | grep horizon

    example:

    [root@aio1 ~]# lxc-ls -f | grep horizon  aio1_horizon_container-b2daccaa RUNNING 1 onboot, openstack 10.255.255.213, 172.29.239.229 - false
    ```

2. Enter horizon container:

    ```text
    [root@aio1 ~]# lxc-attach aio1_horizon_container-b2daccaa
    ```

3. Install requirements packages:

    ```text
    root@aio1-horizon-container-b2daccaa:~# apt install python3-yaml git -y
    ```

4. Clone from github instalations files:

    ```text
    root@aio1-horizon-container-b2daccaa:~# git clone https://github.com/Storware/openstack-horizon-ui-vprotect-extensions
    ```

5. Change owner of the plugin directory to horizon:horizon

    ```text
    root@aio1-horizon-container-b2daccaa:~# chown -R horizon:horizon openstack-horizon-ui-vprotect-extensions
    ```

6. Enter plugin directory:

    ```text
    root@aio1-horizon-container-b2daccaa:~# cd openstack-horizon-ui-vprotect-extensions
    ```

7. Optionally you can ping vprotect server by ping

    ```text
    root@aio1-horizon-container-b2daccaa:~# ping vprotect-server-IP-ADDRESS
    ```

8. Next, install the plugin

    ```text
    root@aio1-horizon-container-b2daccaa:~# python3 install.py http://vprotect-ip:8080/api admin_user admin_password
    ```

    When the installation process is completed, plugin files should be placed in
    /usr/share/openstack-dashboard/openstack_dashboard directory. If your path to dashboard directory is different, create symbolic links from plugin install directories to non-standard directories.

    **Example:**
    ```text
    root@aio1-horizon-container-b2daccaa:~# ln -s /usr/share/openstack-dashboard/openstack_dashboard/dashboards/vprotect /openstack/venvs/horizon-23.1.0.dev65/lib/python3.8/dist-packages/openstack_dashboard/dashboards/

    root@aio1-horizon-container-b2daccaa:~#  ln -s /usr/share/openstack-dashboard/static/vprotect /openstack/venvs/horizon-23.1.0.dev65/lib/python3.8/dist-packages/static/

    root@aio1-horizon-container-b2daccaa:~#  ln -s /usr/share/openstack-dashboard/openstack_dashboard/enabled/_50_vprotect.py /openstack/venvs/horizon-23.1.0.dev42/lib/python3.8/dist-packages/openstack_dashboard/enabled/
    ```

9. Edit /etc/apache2/sites-available/openstack-dashboard.conf file:
   *  Add alias for static files
   ```text
   Alias /dashboard/static /openstack/venvs/horizon-23.1.0.dev65/lib/python3.8/dist-packages/static/
   ```
   * Directory tag informs you, where dashboards directories should be placed.
   * Second Directory tag informs where static directory from plugin should be placed


   Example configuration file should look like this:


   ```text
   # Ansible managed

   # If horizon is being served via SSL from this web server,
   # then we must redirect HTTP requests to HTTPS.

   # If horizon is being served via SSL via a load balancer, we
   # need to listen via HTTP on this web server. If SSL is not
   # enabled, then the same applies.
   <VirtualHost 172.29.239.229:80>
   ServerName aio1-horizon-container-b2daccaa.openstack.local
   LogLevel info
   ErrorLog syslog:daemon
   CustomLog "|/usr/bin/env logger -p [daemon.info](http://daemon.info/) -t apache2" "%h %l %u \"%r\" %>s %b \"%{Referer}i\" \"%{User-agent}i\""
   Options +FollowSymLinks
   RequestHeader set X-Forwarded-Proto "https"

   WSGIScriptAlias / /openstack/venvs/horizon-23.1.0.dev65/lib/python3.8/dist-packages/openstack_dashboard/wsgi.py
   WSGIDaemonProcess horizon user=horizon group=horizon processes=1 threads=1 python-path=/openstack/venvs/horizon-23.1.0.dev65/lib/python3.8/site-packages

   WSGIProcessGroup horizon
   WSGIApplicationGroup %{GLOBAL}

   <Directory /openstack/venvs/horizon-23.1.0.dev65/lib/python3.8/dist-packages/openstack_dashboard>
   <Files wsgi.py >
   <IfVersion < 2.4>
   Order allow,deny
   Allow from all
   </IfVersion>
   <IfVersion >= 2.4>
   Require all granted
   </IfVersion>
   </Files>
   </Directory>

   Alias /static /openstack/venvs/horizon-23.1.0.dev65/lib/python3.8/dist-packages/static/
   Alias /dashboard/static /openstack/venvs/horizon-23.1.0.dev65/lib/python3.8/dist-packages/static/

   <Directory /openstack/venvs/horizon-23.1.0.dev65/lib/python3.8/dist-packages/static/>
   Options -FollowSymlinks
   <IfVersion < 2.4>
   AllowOverride None
   Order allow,deny
   Allow from all
   </IfVersion>
   <IfVersion >= 2.4>
   Require all granted
   </IfVersion>
   </Directory>
   </VirtualHost>
   ```

10. Edit /openstack/venvs/horizon-23.1.0.dev65/lib/python3.8/dist-packages/openstack_dashboard/urls.py and add in urlPatterns following line
    ```text
    url(r'^dashboard/', horizon.base._wrapped_include(horizon.urls))
    ```
    Your urls.py should looks like:

    ```text
    """
    URL patterns for the OpenStack Dashboard.
    """

    from django.conf import settings
    from django.conf.urls import include
    from django.conf.urls.static import static
    from django.conf.urls import url
    from django.contrib.staticfiles.urls import staticfiles_urlpatterns
    from django.views import defaults

    import horizon
    import horizon.base
    from horizon.browsers import views as browsers_views
    from horizon.decorators import require_auth

    from openstack_dashboard.api import rest
    from openstack_dashboard import views

    urlpatterns = [
    url(r'^$', views.splash, name='splash'),
    url(r'^api/', include(rest.urls)),
    url(r'^header/', views.ExtensibleHeaderView.as_view()),
    url(r'', horizon.base._wrapped_include(horizon.urls)),
    **url(r'^dashboard/', horizon.base._wrapped_include(horizon.urls)),**
    ]

    # add URL for ngdetails
    ```

11. Restart httpd service 
    ```text
    /etc/init.d/apache2 restart
    ```

12. After refreshing dashboard site you should see vProtect button in Openstack menu.