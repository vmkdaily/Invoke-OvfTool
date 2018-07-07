## For more detail see the supporting blog post at:
https://vmkdaily.ghost.io/using-powershell-and-ovftool-to-deploy-vcenter-server-appliance-on-esxi/

The following is the current cmdlet help:

    PS>help Invoke-OvfTool -Full

    NAME
        Invoke-OvfTool

    SYNOPSIS


    SYNTAX
        Invoke-OvfTool [[-Path] <String>] [[-OvfConfig] <PSObject>] [[-TemplatePath] <String>] [[-OutputPath] <String>]
        [[-JsonPath] <String>] [[-Mode] <String>] [[-Dos2UnixPath] <String>] [-SkipDos2Unix] [[-LogDir] <String>]
        [[-Description] <String>] [[-Depth] <Int32>] [<CommonParameters>]


    DESCRIPTION
        Deploy a VMware vCenter Server using Microsoft PowerShell. In the vCenter Server ISO, there is a complete folder-based layout of tools
        that support the deployment of OVA. The binary is known as OVFTool and is available for many devices. Here we focus on Windows as our
        client that we will deploy from.

        To use this kit, download and extract the vCenter Server ISO to a directory, using a POSIX compliant unzipper such as 7zip.
        By default, we expect it to be in the Downloads folder (i.e. "$env:USERPROFILE\Downloads"). However, you can also populate the Path
        parameter with the location to the uncompressed bits.

        Note: You may see mention of 32bit because that is how OVFTOOL works. However, all binaries support 64 bit Windows.

        Important: Currently a utility called dos2unix.exe is also required to get the JSON file in the proper unix format.
        Much like the VC binaries, dos2unix.exe is a folder-based runtime, so there is no need to install. Simply download it
        and populate the Dos2UnixPath parameter with the full path. By default we expect it to be in "$env:USERPROFILE\Downloads",
        like everything else.

        Download 7zip:
        https://www.7-zip.org/download.html

        Download dos2unix:
        https://sourceforge.net/projects/dos2unix/files/dos2unix/

        Download vCenter Server (requires login; Create account if needed):
        https://my.vmware.com/group/vmware/details?downloadGroup=VC670B&productId=742&rPId=24515


    PARAMETERS
        -Path <String>
            String. The path to the win32 directory of the extracted vCenter Server installation ISO.
            By default we expect "$env:USERPROFILE\Downloads\VMware-VCSA-all-6.7.0-8832884\vcsa-cli-installer\win32"

            Required?                    false
            Position?                    1
            Default value                "$env:USERPROFILE\Downloads\VMware-VCSA-all-6.7.0-8832884\vcsa-cli-installer\win32"
            Accept pipeline input?       false
            Accept wildcard characters?  false

        -OvfConfig <PSObject>
            PSObject. A hashtable containing the deployment options for a new vCenter Server appliance. See the help for details on creating and using a variable for this purpose.

            Required?                    false
            Position?                    2
            Default value
            Accept pipeline input?       false
            Accept wildcard characters?  false

        -TemplatePath <String>
            String. Path to the JSON file to model after. This would be the example file provided by VMware or one that you customized previously to become your master.
            We assume no previous work was done and we use the template from VMware and modify as needed.
            The default is "$env:USERPROFILE\Downloads\VMware-VCSA-all-6.7.0-8832884\vcsa-cli-installer\templates\install\embedded_vCSA_on_ESXi.json".

            Required?                    false
            Position?                    3
            Default value                "$env:USERPROFILE\Downloads\VMware-VCSA-all-6.7.0-8832884\vcsa-cli-installer\templates\install\embedded_vCSA_on_ESXi.json"
            Accept pipeline input?       false
            Accept wildcard characters?  false

        -OutputPath <String>
            String. The full path to the JSON configuration file to create. If the file exists, we overwrite it.
            The default is "$env:Temp\myConfig.JSON".

            Required?                    false
            Position?                    4
            Default value                "$env:Temp\myConfig.JSON"
            Accept pipeline input?       false
            Accept wildcard characters?  false

        -JsonPath <String>
            String. The full path to the JSON configuration file to use when deploying a new vCenter appliance.

            Required?                    false
            Position?                    5
            Default value
            Accept pipeline input?       false
            Accept wildcard characters?  false

        -Mode <String>
            String. Tab complete through options of Design, Test, Deploy or LogView.

            Required?                    false
            Position?                    6
            Default value
            Accept pipeline input?       false
            Accept wildcard characters?  false

        -Dos2UnixPath <String>
            String. Dos2Unix binary location. Adjust as needed and download if you do not have it.

            Required?                    false
            Position?                    7
            Default value                "$env:USERPROFILE\Downloads\dos2unix-7.4.0-win64\bin\dos2unix.exe"
            Accept pipeline input?       false
            Accept wildcard characters?  false

        -SkipDos2Unix [<SwitchParameter>]
            Switch. Optionally, activate to skip all dos2unix requirements and file conversion steps.

            Required?                    false
            Position?                    named
            Default value                False
            Accept pipeline input?       false
            Accept wildcard characters?  false

        -LogDir <String>
            String. The directory to write or read logs related to ovftool. This is not PowerShell transcript logging, this is purely deployment related and the resuling output paths are long, so keep this path short for best results.

            Required?                    false
            Position?                    8
            Default value                $env:Temp
            Accept pipeline input?       false
            Accept wildcard characters?  false

        -Description <String>
            String. The name of the site or other friendly identifier for this job.

            Required?                    false
            Position?                    9
            Default value
            Accept pipeline input?       false
            Accept wildcard characters?  false

        -Depth <Int32>
            Integer. Optionally, enter an integer value denoting how many objects to support when importing a JSON template.
            The default is '10', which is up from the Microsoft default Depth of '2'. The maximum is 100.  The Depth must be
            higher than the number of items in the JSON template that we read in.

            Required?                    false
            Position?                    10
            Default value                10
            Accept pipeline input?       false
            Accept wildcard characters?  false

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    INPUTS

    OUTPUTS

    NOTES


            Name:         Invoke-OvfTool.ps1
            Author:       Mike Nisk
            Dependencies: Extracted vCenter Server installation ISO for the latest vCenter Server 6.7
            Dependencies: dos2unix.exe (not provided by VMware). This is needed to convert the OutputJson file into unix filetype
            Dependencies: Account on ESXi that can be used for login and deployment. The recommendation is to create an account for
                          the duration of the deployment and then remove it. In the examples, we refer to a ficticious account called
                          ovauser which you can manually create on ESXi as a local user. After the OVA is deployed you can remove the user.
                          Creating and removing ESXi users is optional and is not handled by the script herein. Alternatively, just use root.

        -------------------------- EXAMPLE 1 --------------------------

        PS C:\>#Paste this into PowerShell

        $OvfConfig = @{
          esxHostName            = "esx01.lab.local"
          esxUserName            = "root"
          esxPassword            = "VMware123!!"
          esxPortGroup           = "VM Network"
          esxDatastore           = "datastore1"
          ThinProvisioned        = $true
          DeploymentSize         = "tiny"
          DisplayName            = "vcsa01"
          IpFamily               = "ipv4"
          IpMode                 = "static"
          Ip                     = "10.100.1.201"
          FQDN                   = "vcsa01.lab.local"
          Dns                    = "10.100.1.10"
          SubnetLength           = "24"
          Gateway                = "10.100.1.1"
          VcRootPassword         = "VMware123!!!"
          VcNtp                  = "0.pool.ntp.org, 1.pool.ntp.org,2.pool.ntp.org,3.pool.ntp.org"
          SshEnabled             = $true
          ssoPassword            = "VMware123!!!"
          ssoDomainName          = "vsphere.local"
          ceipEnabled            = $false
        }
        
        Note: Passwords must be complex.
        
        This example created a PowerShell object to hold the desired deployment options.
        Please note that some values are case sensitive (i.e. datastore).


        -------------------------- EXAMPLE 2 --------------------------

        PS C:\>$Json = Invoke-OvfTool -OvfConfig $OvfConfig -Mode Design
        $Json  | fl *  #observe output and get the path
        Invoke-OvfTool -OvfConfig $OvfConfig -Mode Deploy -JsonPath <path-to-your-json-file>

        This example creates a variable pointing to a default VMware JSON configuration.
        We then overlay our settings at deploy time using the $OvfConfig variable we created previously.
        This results in a customized vCenter Appliance.




        -------------------------- EXAMPLE 3 --------------------------

        PS C:\>$result = Invoke-OvfTool -Mode LogView -LogDir "c:\Temp\workflow_1525282021542"

        $result            # returns brief overview of each log file
        $result |fl *      # returns all detail

        This example shows how to review logs from previous runs. If you do not specify Logir parameter, we search for all JSON files in the default LogDir location.

        ABOUT WINDOWS CLIENT REQUIREMENTS

          It is recommended that you have already run the test script that VMware includes to
          check for the required 32bit C++ runtime package:

            vcsa-cli-installer\win32\check_windows_vc_redist.bat

          If the above script indicates that you are out of date, the minimum required version
          is included on the vCenter Server ISO. You can also download the latest version directly
          from Microsoft.com.


        ABOUT SSL CERTIFICATE HANDLING

          When using vcsa-deploy.exe (which we call in the background), one can optionally set a preference at runtime
          to determine how invalid certificates are handled. The "--no-esx-ssl-verify" is deprecated and "--no-ssl-certificate-verification"
          is used instead.

        ABOUT UNICODE ESCAPE (u0027)

          When dealing with JSON files in PowerShell you may notice the characters u0027 accidentally placed throughout your text content.
          This is a known issue and we handle it. We prevent these unicode escape characters (u0027) from being injected into the outputted
          JSON file by adjusting the Depth parameter of ConvertTo-Json.

          Over time, and depending on the deployment options required, you may need to adjust the Depth to suit your needs.
          By keeping the default depth of 2, you will notice 'u0027' throughout your JSON configuration file.

          To avoid this, we attempt to increase the Depth to something greater than the total count of sections VMware currently provides in the JSON template.
          The Microsoft supported maximum for PowerShell 5.1 is a Depth of 100, or 100 items that can be ported in as objects. For our purposes, in doing
          an ESXi deployment of an embedded VC, we only need a Depth of '4' or '5'. However, you can safely make it something like 50 or 99 without issue.

          More about unicode escape:
          http://www.azurefieldnotes.com/2017/05/02/replacefix-unicode-characters-created-by-convertto-json-in-powershell-for-arm-templates/


        ABOUT UTF8 REQUIREMENTS (and dos2unix.exe)

          When saving the JSON file with PowerShell's Out-File cmdlet, we encode using utf8 and then run dos2unix.exe (with the -o parameter)
          to ensure that the file is encoded as unix utf8. If you skip this final step of running dos2unix, the VMware pre-deployment tests may fail.

        More on SSL Errors
          For ovatool contained in the latest vCenter 6.7 build 8832884, the command parameter '--no-esx-ssl-verify' is deprecated and
          you must use the new parameter '--no-ssl-certificate-verification' instead.


