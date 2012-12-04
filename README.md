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
    function() {alert('Create Shortcut Success')}, // Success function
    function() {alert('Create Shortcut failed')} // Failure function
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


<h1>Source files :</h1>
1.ExampleActivity.java
<pre>
package org.phonegap.emp;

import org.apache.cordova.DroidGap;

import android.os.Bundle;

public class ExampleActivity extends DroidGap {
    /** Called when the activity is first created. */
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        super.loadUrl("file:///android_asset/www/index.html");
        //GlobalConfig is a defined java class
        GlobalConfig.CUR_ACTIVITY_CLASSNAME = this.getLocalClassName();
    }
}
</pre>
2.DeskTopShortCut.java
<pre>
package org.phonegap.plugins.deskshortcut;

import org.apache.cordova.api.CallbackContext;
import org.apache.cordova.api.CordovaPlugin;
import org.apache.cordova.api.LOG;
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;
import org.phonegap.emp.GlobalConfig;
import org.phonegap.emp.R;

import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.Intent.ShortcutIconResource;

public class DeskTopShortCut extends CordovaPlugin {
    
	@Override
	public boolean execute(String action, JSONArray args,CallbackContext callbackContext) throws JSONException {
		try{
			JSONObject jo = args.getJSONObject(0);
			String name = jo.getString("name");
			addShortcut(name);
			LOG.d("ShortCut","Create Short Succcess!");
		}catch(Exception e){
			LOG.d("ShortCut","Create Short Failed!"+e.toString());
			return false;
		}
		return true;
	}
	private Context context;
	private void addShortcut(String shortName){  
		context = cordova.getActivity().getApplicationContext();
	    Intent shortcut = new Intent("com.android.launcher.action.INSTALL_SHORTCUT");  
	           
	    shortcut.putExtra(Intent.EXTRA_SHORTCUT_NAME, shortName);  
	    shortcut.putExtra("duplicate", false); 
	           
	    ComponentName comp = new ComponentName(context.getPackageName(), "."+GlobalConfig.CUR_ACTIVITY_CLASSNAME);  
	    shortcut.putExtra(Intent.EXTRA_SHORTCUT_INTENT, new Intent(Intent.ACTION_MAIN).setComponent(comp));  
	   
	    ShortcutIconResource iconRes = Intent.ShortcutIconResource.fromContext(context, R.drawable.notification);  
	    shortcut.putExtra(Intent.EXTRA_SHORTCUT_ICON_RESOURCE, iconRes);  
	    context.sendBroadcast(shortcut);  
	}  
}

</pre>


3.DeskShortCut.js
<pre>
var DeskTopShortCut = function() {};
            
DeskTopShortCut.prototype.create = function(content, success, fail) {
    return cordova.exec( function(args) {
        success(args);
    }, function(args) {
        fail(args);
    }, 'DeskTopShortCut', '', [content]);
};

if(!window.plugins) {
    window.plugins = {};
}
if (!window.plugins.DeskTopShortCut) {
    window.plugins.DeskTopShortCut = new DeskTopShortCut();
}
</pre>
