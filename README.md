DesktopShortcut plugin for PhoneGap
===============

The DesktopShortcut plugin allows you to create a shortcut to the desktop by your PhoneGap application.

<b>Adding the Plugin to your project</b>

1.PhoneGap 1.0 and later

2.Add java code to your project's build source

3.Register the plugin in the <b>plugins.xml</b>(android:res/xml/config.xml) file

<plugin name="DeskTopShortCut" value="org.phonegap.plugins.deskshortcut.DeskTopShortCut"/>

4.Call the plugin, specifying name, success function, and failure function<br/>
<pre>
window.plugins.DeskTopShortCut.create({
    name: 'my shortcut'},
    function() {}, // Success function
    function() {alert('Share failed')} // Failure function
);
</pre>


5.In the <b>AndroidManifest.XML</b> documents state create and delete shortcut statement when the authority
<pre>
&lt;uses-permission android:name="com.android.launcher.permission.INSTALL_SHORTCUT" /&gt;  
&lt;uses-permission android:name="com.android.launcher.permission.UNINSTALL_SHORTCUT" /&gt;
</pre>
6.To install the plugin, move statusbarnotification.js to your project's www folder and include a reference to it in your html file after cordova.js.
<pre>
&lt;script type="text/javascript" charset="utf-8" src="cordova.js">&lt;/script&gt;
&lt;script type="text/javascript" charset="utf-8" src="DeskShortCut.js">&lt;/script&gt;
</pre>
