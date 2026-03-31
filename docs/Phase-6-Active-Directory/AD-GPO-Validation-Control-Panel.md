# 🧪 Active Directory GPO Validation: Control Panel Restriction

## 📌 Objective
To validate the Active Directory deployment pipeline by creating, deploying, and verifying a Group Policy Object (GPO) that restricts user access to the Control Panel and Windows Settings across the domain.

## 🛠️ Step 1: Create the GPO on the Domain Controller
Execute the following steps on the Windows Server 2019 Domain Controller to create the baseline policy:

1. Open **Server Manager**.
2. In the top right corner, click **Tools** -> **Group Policy Management**.
3. In the left pane, expand **Forest: ad.lab** -> **Domains** -> **ad.lab**.
4. Right-click on your domain (`ad.lab`) and select **"Create a GPO in this domain, and Link it here..."**.
5. Name the new policy `Test-Block-Control-Panel` and click **OK**.

## ⚙️ Step 2: Configure the Policy Constraints
Configure the newly created GPO to enforce the target restrictions:

1. Right-click your new `Test-Block-Control-Panel` policy and select **Edit**. (This opens the Group Policy Management Editor).
2. In the left pane, drill down into this exact path:
   `User Configuration -> Policies -> Administrative Templates -> Control Panel`
3. In the right pane, find the setting named **"Prohibit access to Control Panel and PC settings"**.
4. Double-click it, change the toggle from "Not Configured" to **Enabled**, and click **OK**.
5. Close the Group Policy Management Editor.

> **Note:** The rule is now live on the server, but your Windows 10 machine will not pull it down on its own for another 90 minutes.

## 💻 Step 3: Force the Client Update
Execute the following on the Windows 10 Client VM to manually pull the new policy. Ensure you are logged in with a Domain account, not a local offline account.

1. Click the Start button, type `cmd`, and hit **Enter**.
2. Type the following command and hit **Enter**:

   ```cmd
   gpupdate /force
   ```

3. Wait a few seconds for the confirmation messages:
   * *"Computer Policy update has completed successfully"*
   * *"User Policy update has completed successfully"*

## ✅ Step 4: Verification Test
Confirm that the restriction successfully propagated across the network:

1. Click the Windows Start button.
2. Click the gear icon (**Settings**), or type **Control Panel** and try to open it.
3. **Expected Result:** You should receive a red error box stating:
   > *"This operation has been cancelled due to restrictions in effect on this computer. Please contact your system administrator."*
