🧪 Active Directory GPO Validation: Control Panel Restriction
📌 Objective
To validate the Active Directory deployment pipeline by creating, deploying, and verifying a Group Policy Object (GPO) that restricts user access to the Control Panel and Windows Settings across the domain.

🛠️ Step 1: Create the GPO on the Domain Controller
Execute the following steps on the Windows Server 2019 Domain Controller to create the baseline policy:

Open Server Manager.

In the top right corner, click Tools -> Group Policy Management.

In the left pane, expand Forest: ad.lab -> Domains -> ad.lab.

Right-click on your domain (ad.lab) and select "Create a GPO in this domain, and Link it here...".

Name the new policy Test-Block-Control-Panel and click OK.

⚙️ Step 2: Configure the Policy Constraints
Configure the newly created GPO to enforce the target restrictions:

Right-click your new Test-Block-Control-Panel policy and select Edit. (This opens the Group Policy Management Editor).

In the left pane, drill down into this exact path:
User Configuration -> Policies -> Administrative Templates -> Control Panel

In the right pane, find the setting named "Prohibit access to Control Panel and PC settings".

Double-click it, change the toggle from "Not Configured" to Enabled, and click OK.

Close the Group Policy Management Editor.

Note: The rule is now live on the server, but your Windows 10 machine will not pull it down on its own for another 90 minutes.

💻 Step 3: Force the Client Update
Execute the following on the Windows 10 Client VM to manually pull the new policy. Ensure you are logged in with a Domain account, not a local offline account.

Click the Start button, type cmd, and hit Enter.

Type the following command and hit Enter:

DOS
gpupdate /force
Wait a few seconds for the confirmation messages:

"Computer Policy update has completed successfully"

"User Policy update has completed successfully"

✅ Step 4: Verification Test
Confirm that the restriction successfully propagated across the network:

Click the Windows Start button.

Click the gear icon (Settings), or type Control Panel and try to open it.

Expected Result: You should receive a red error box stating:

"This operation has been cancelled due to restrictions in effect on this computer. Please contact your system administrator."
