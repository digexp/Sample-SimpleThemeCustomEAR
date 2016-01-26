# IBM Simple Theme Custom EAR File Creation

The Simple Theme is a simplified theme which can be used as a starting point for the development of custom themes. It is included in Portal 8.5 starting with CF08.

Sometimes it may be necessary to modify the jsps which are used with the Simple Theme. To do this, you would need to create and install a new ear file so you can make your modifications there. To facilitate this, we have created a paa which does the work for you.

The SimpleThemeEar paa allows you to quickly make a copy of the Simple Theme ear files and install your own custom version. This will allow you to change the jsps and dynamic content spots in the theme.

## Instructions
1. Copy the paa to your Portal server. In the example below, the file was copied to the /tmp/paa/ directory.
2. To install the paa, open a command shell and go to the ConfigEngine directory, for example: /opt/WebSphere/wp_profile/ConfigEngine
3. Run these two tasks, remembering to provide the appropriate passwords:

        ./ConfigEngine.sh install-paa -DPAALocation=/tmp/paa/SimpleThemeEar.paa -DWasPassword=WAS_PASSWORD -DPortalAdminPwd=PORTAL_PASSWORD
        ./ConfigEngine.sh deploy-paa -DappName=SimpleThemeEar -DWasPassword=WAS_PASSWORD -DPortalAdminPwd=PORTAL_PASSWORD
4. Create an xml file called ExportTheme.xml. Its contents should look like this:

        <?xml version="1.0" encoding="UTF-8"?>
        <request xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="PortalConfig_7.0.0.xsd" type="export" create-oids="true">
            <portal action="locate">
                <theme action="export" uniquename="*" />
            </portal>
        </request>
5. Access your Portal server from a browser and open the Applications menu, select Theme Development
6. Open the Theme Properties dialog for your custom simple theme, and select Advanced
7. Copy the Unique Name to the clipboard and replace the * in the ExportTheme.xml file with that name.
8. Use xmlaccess to execute the ExportTheme.xml file. You can either use the command line interface, or in your Portal browser, go to Administration > Portal Settings > Import XML.  This should generate an output file with your theme definition, you can name it ThemeDef.xml.
9. Within the ThemeDef.xml, find the theme definition element (<theme ... >) and replace the "context-root" attribute with the value "/customSimpleTheme." 
10. Use xmlaccess to execute the ThemeDef.xml. This will update the theme definition to ensure it is using the newly deployed EAR file.
11. Edit your theme profile and replace the "wp_dyncs_simple" module with "custom_dyncs_simple." This ensures the dynamic content spots are using JSPs in the newly deployed EAR file instead of the default.
12. Now you can update the dynamic content spot JSPs as needed.  The PAA deployed the JSPs in the system to this location:
/wp_profile/installedApps/<cell>/Custom_SimpleTheme.ear  
 
