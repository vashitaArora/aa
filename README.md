    Severity classification is as follows. **All Sev0 and Sev1 bugs should have a root cause in the associated bug
        Sev 0 - Any failure of VSO.CI or VSO.Release.CI to build successfully or anything causing a failure to deploy in P0 runs
            Example: build break in VSO.CI or failing VSSDF step in a P0 run.
        Sev 1 - Any consistent failure of a P0 run where deployment succeeds or a Sev2 with 10 or more occurences.
            Example: A consistent failure of one or more tests in a P0 run.
        Sev 2 - Identification and mitigation of a flaky (intermittent) failure of a test in P0 run or VSO.PR.
            Example: A test begins to fail intermittently in SPS.SelfTest or someone reports a L0 test failure in their PR unrelated to their changes.
        Sev 3 - Anything found in FlakeyTestDetector


<noinclude>
{{Template:Playbook Header|Owners=chrisid|WGSponsor=[mailto:chrisid chrisid]|DocumentQuality=InProgress}}
</noinclude>
[\<\< Return to Skyrise V2 Home](Skyrise-V2 )

This is a continuation of the [Getting Started with Build (Skyrise V2)](Getting-Started-with-Build-\(Skyrise-V2\) ) walkthrough which walks you through the Skyrise V2 engineering system.

## Prepare your machine for deployment

  - If you have been using your machine with the Skyrise V1 environment, it should be ready to go without any additional action.
  - Follow the steps [here](Preparing-your-environment-\(Skyrise-V2\)#Prepare-your-machines-for-Deployment ) if you're using a clean machine.
  - If you plan to deploy your Service dependencies in containers using [VSSC](VSSC ), you'll need to install Docker for Windows. See [VSSC](VSSC ) for more information.

## Deploy your Service Dependencies

In most cases you will need to deploy at least SPS and TFS in addition to the Service(s) that your team works on. You may have other Service dependencies that need to be deployed as well. You have two options here:

  - Deploy your dependencies in containers using [VSSC](VSSC ).
  - Deploy your dependencies from locally built binaries using VSSDF.

To deploy your dependencies from locally built binaries:

  - [Start the Build Environment](Getting-Started-with-Build-\(Skyrise-V2\)#Start-the-Skyrise-V2-build-environment-.28CMD.exe.29 ).
  - [Build the product](Getting-Started-with-Build-\(Skyrise-V2\)#Run-your-first-build ).
  - Once the build has finished, from an elevated build environment window (CMD.exe or PowerShell), use vssdf to deploy your Service dependencies. For example:

`   vssdf sps.l2 tfs.l2 `

  - This will:
      - Copy SPS and TFS bits to the C:\\LR\\sps.l2 and C:\\LR\\tfs.l2 folders using VSSI.
      - Copy SPS and TFS L2 tests to {ENLISTMENT\_ROOT}\\Tests.
      - Deploy sps.l2 and tfs.l2 using LightRail.

If you have additional Service dependencies, follow the same pattern. For example, to deploy SPS, TFS, Gallery, and EMS:

`   vssdf sps.l2 tfs.l2 gallery.l2 ems.l2`

**It is now necessary to explicitly deploy SpsExtension to enable profile scenarios and Aex to enable SPS UI if those are required for you.**

`   vssdf sps.l2 tfs.l2 spsext.l2 aex.l2`

## Deploy your Service(s)

To deploy your own Service(s), use the same pattern. For example, to deploy CodeLens:

`   vssdf cl.l2`

You can also, of course, deploy your Dependencies and your own Service(s) all at the same time. For example:

`   vssdf sps.l2 tfs.l2 gallery.l2 ems.l2 cl.l2`

Once SPS and TFS successfully deploys you can browse the deployment by opening up Internet Information Services (IIS) Manager on your machine to find the URL for the sites. This can be found under <Machine Name> -\> Sites. (TFS is typically deployed under 127.0.0.1:81)

To create an account after SPS and TFS install you can

1.  Navigate to SPS Home page which will allow you to create an account
2.  Use the LR cmdlet in SPS Create-TestAccountAndProfile -AccountName "MyAccount"

<!-- end list -->

  - Before you can run that commandlet, you'll need to CD .\\Sps\\DevFabric after launching LightRail Sps (the shortcut is on your desktop)

<li>

Use LR cmdlet in SPS Create-OrganizationCollection -OrganizationName "MyOrg" -CollectionName "MyCollection" -CollectionOwner (user Id guid) -PreferredRegion "SCUS"

</li>

</ol>

## Deploy TFS to OnPrem from a local build

  - [Start the Build Environment](Getting-Started-with-Build-\(Skyrise-V2\)#Start-the-Skyrise-V2-build-environment-.28CMD.exe.29 ).
  - [Build the product](Getting-Started-with-Build-\(Skyrise-V2\)#Run-your-first-build ).
  - Once the build has finished, from an elevated build environment window (CMD.exe or PowerShell), run the following commands.

[`tfat`](tfat )

  - If you built release bits, call tfat.cmd with the /release option, which will both register a product key for you and pass /configuration:Release to vssi.cmd.

`tfat /release`

  - Tests will also be laid down at the same time, in {ENLISTMENT\_ROOT}\\Tests. There is no need to run DeployTests.

## Basic smoke test - create an account and project

  - For DevFabric
      - Navigate to <https://sps1.vssps.vsts.me/>.
      - Follow the prompts to create an account and project

<!-- end list -->

  - For OnPrem
      - Navigate to <http://localhost:8080/tfs>.
      - Connect from Visual Studio (Team -\> Manage Connections)

## Building and deploying on separate machines

### Recommended method

You're less likely to hit issues using this method.

**On your build machine**

1.  [Start the Build Environment](Getting-Started-with-Build-\(Skyrise-V2\)#Start-the-Skyrise-V2-build-environment-.28CMD.exe.29 ).
2.  [Build the product](Getting-Started-with-Build-\(Skyrise-V2\)#Run-your-first-build ).

**On your deployment machine - CMD version**

1.  From an elevated CMD command prompt, pushd to the UNC path for your src directory. Example:

pushd \\\\yourMachineName\\D$\\VSO\\src

<li>

Initialize this Apex window. If you get nuget package error, try again.

</li>

`init.cmd`

<li>

Deploy using one of the commands described above. Example:

</li>

`vssdf sps.l2 [gallery.l2 ems.l2] tfs.l2`

</ol>

**On your deployment machine - Powershell version**

1.  From an elevated Powershell prompt, allow execution of remote unsigned scripts for this Powershell session:

Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process -Force

<li>

Map your remote drive to a local drive:

</li>

`subst Z: '\\yourBuildMachine\D$'`

<li>

cd to your source directory using the mapped drive.

</li>

`cd Z:\VSO\src`

<li>

Initialize this Apex window.

</li>

`.\init.ps1`

<li>

Deploy using one of the commands described above. Example:

</li>

`vssdf sps.l2 [gallery.l2 ems.l2] tfs.l2`

</ol>

#### Troubleshooting

If init.cmd results in an error, it may be there are nuget packages that need to be retrieved. Run init.cmd on your build machine and then re-attempt init.cmd in a new window on the deployment machine.

If init.ps1 results in an error, try the CMD version to determine if the issue is specific to the Powershell environment.

### Bin-dir-only method

If you have a requirement to avoid accessing the source tree, you can use this method instead of the recommended method above. You are more likely to encounter issues or gotchas with this method.

**On your build machine**

1.  [Start the Build Environment](Getting-Started-with-Build-\(Skyrise-V2\)#Start-the-Skyrise-V2-build-environment-.28CMD.exe.29 ).
2.  [Build the product](Getting-Started-with-Build-\(Skyrise-V2\)#Run-your-first-build ).
3.  Optional: Transfer your bin directory to the machine you're deploying on, say a Nebula or Azure VM.

**On your deployment machine - CMD version**

1.  From an elevated command prompt, go into your bin\\scripts directory.
      - If you copied your bin directory locally:
    cd D:\\transfers\\bin\\scripts
2.  Otherwise, pushd to the UNC path for your build machine:

pushd \\\\yourBuildMachine\\D$\\VSO\\bin\\scripts

</ul>

</li>

<li>

Deploy using one of the commands described above. Example:

</li>

` vssdf sps.l2 [gallery.l2 ems.l2] tfs.l2`

</ol>

## Tips

  - If you want to update your bits without dropping your databases and redeploying, use vssi instead of vssdf.

DevFabric:

`vssi tfs.l2`

OnPrem:

`vssi TfsOnPrem`

Client:

`vssi TfsClient`

TestExplorer:

`vssi TestExplorer`

  - If you need to run a LightRail command, look on your desktop for LightRail shortcuts after running vssdf or vssi for the first time.

## Troubleshooting

If vssi hangs try using the verbose command to get more details:

`vssi tfsOnPrem /verbose`

### Additional install arguments

The deploy scripts (`vssdf`, `tfat`, `tfproxy`, `oi`, etc) all call `VssInstall.exe` via `vssi.cmd`. You can supply additional arguments to `VssInstall.exe` via the `VSSINSTALL_ARGUMENTS` environment variable. For example, if you want to deploy release bits:

cmd:

    set VSSINSTALL_ARGUMENTS=/configuration:Release

PowerShell:

    $env:VSSINSTALL_ARGUMENTS="/configuration:Release"

After you've set the environment variable, run the deploy commands.\*

For `vssi`, you can skip the environment variable if you like and pass the additional arguments directly.

    vssi TfsOnPrem "/configuration:Release"

This applies to `vssdf` as well, as long as the additional arguments don't include an equal sign. The `vssdf` batch script will replace the equal sign with a space before calling `vssi`.

\*Note about the example above: For `tfat`, setting VSSINSTALL\_ARGUMENTS to deploy release bits is insufficient. Use tfat's /release option instead which will both register a product key and install release bits. You have forgotten the /release switch if you are attempting to deploy release but get the error `[Error] [Prepare Configuration] TF400094: Registering the trial version key failed.`

## Debugging

Attaching the debugger to specific VSTS services can be simplified by using the internal VS IDE extension QuickAttach. QuickAttach removes the need to attach to all w3wp.exe instances to debug a single service (or use Process Explorer to discover which instance is hosting your service).

A .vsix installation package is available here:

    \\vscsstor\Users\vinma\QuickAttach\QuickAttach.vsix

The source code is hosted at [<https://mseng.visualstudio.com/InternalTools/_git/QuickAttach>](https://mseng.visualstudio.com/InternalTools/_git/QuickAttach?_a=readme) (along with further details on the features). Anyone is welcome to contribute to the extension.

[Category:Skyrise V2](Category:Skyrise-V2 )

