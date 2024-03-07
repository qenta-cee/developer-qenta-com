# SDK and Tools

**%product_name%** optimizes the integration experience through its severals tools for developers.

## Configuration

To use the SDK or the Proxy Server you will need to configure them. The configuration parameters can be obtained from the [%prowallet_name%](%prowallet_url%).


<procedure title="Getting SDK configuration">
<step>
Once you logged into the console. Go to the <code>SDK SETTINGS</code> option in the <code>Manage</code> menu.
<img src="ProWallet_ManageMenu_SDKSettings.png" alt="SDK Settings"/>
</step>
<step>
In the <code>SDK Settings</code> home screen you wil be able to see the following information:

<list>
<li>
<code>User ID</code> : Unique identifier of the user
</li>
<li>
<code>Organization ID</code> : Unique identifier of the organization
</li>
<li>
<code>Access Token</code> : Allow the SDK make request to the <a href="Authentication.md">REST APIs</a>
</li>
<li>
<code>Private key</code> : Allows a client side signature of transactions
</li>
</list>

<img src="ProWallet_SDK_Settings.png" alt="SDK Settings home"/>

<note>The variable values can be used as environment variables or generate a JSON file by clicking on 'DOWNLOAD JSON' button</note>

</step>
<step>
ProWallet will require you user secret before to allow see or copy the <code>Private key</code> value.
<img src="ProWallet_SDK_Settings_GetPrivateKey.png" alt="Get private key"/>
<warning>Private key is used to sign each transaction, avoid share it</warning>
</step>
</procedure>