# Testing OuiSync

Here we'll explain basic steps to test OuiSync. There will be a number of test
scripts ordered by likely something is to fail (least likely to most likely).

It is therefore a good idea to start with the first one and progress to the
next one each time the test succeeds. Alternatively, one can start with a later
test case and then either progress or regress depending on whether the current
test succeeds or fails.

Before you start, please **ensure all devices involved in the test have the same
version**:

* Open the app
* Click on the settings icon (gear wheel in the top right corner)
* Scroll all the way down to see the field `App version`

The following test can be performed either locally where all the devices are connected
to the same WiFI, or globally where all the devices connected through the internet, or
a mix of the two.

At the beginning we'll have one device labeled *C* which will be the one to "create a
repository".  Then we'll have other device labeled *O<sub>i</sub>* into which the
created repository will be "added". Once the repository is added to the other devices
the distinction between who is the creator and who is not will disappear and we'll
call all devices set to sync a single repository *D<sub>i</sub>*.

The steps of the test are as follows:

#### Clean slate

1. Uninstall the OuiSync app if it has been previously installed and install it
   back again on all devices.
2. Note down the `App version` on each device and ensure they are the same.
3. Ensure there are no previoiusly created repositories (they should have been deleted
   by performing the step #1).
4. Note down these other fields in the settings page on both devices:
   * `Connection type`
   * `External IP`
   * `Local IPv4` and `Local IPv6`
   * `Listening on QUIC/UDP IPv4` and `Listening on QUIC/UDP IPv6`

#### Create a repository

5. Hit the "back" button to exit the settings page back into the main page on
   all of the devices.
6. Click on the "Create a Repository" button on device *C*. A dialog shall be shown where you
   need to enter a name and a password for this new repository. This password is unique to
   the device and will not be implicitly shared with other devices.

#### Share the repository

7. Still on device *C*, from the main page, click on the share icon in the top
   right corner.
8. In the `Set permission` field select `Write`. This will allow the other
   devices (*O<sub>i</sub>*) to make changes to the repository.
9. Send the `Share link` presented in the other field from device *C* to all
   *O<sub>i</sub>* devices by either copy/pasting the link to another app
   sharing it directly with another app using the share button.

#### Add the repository

10. On devices *O<sub>i</sub>* copy the `Share link` from the app where you
    received it in and open the OuiSync app.
11. Click the `Add a Shared Repository` button. You'll be presented with a dialog.
12. Paste the `share link` into the first field and give the repository a name and a password
    (these do not need to match the name and password from device *C* and may also be different
    on all *O<sub>i</sub>* device).

The device *C* and all devices into which the `share link` was added into (*O<sub>i</sub>*) are
said to "share the same repository". The distinction about which device
"created" the repository and which device "added" the repository is no longer important. Thus we'll
refer to these devices as just *D<sub>i</sub>*.

#### Check connectivity

13. Go to the setting page and from the click on the button in the field `Connected peers`.

You'll be presented with a table of peers OuiSync knows about. In this table,
the two important columns are the `IP` and the `State`. If the `State` is set
to `Active`, that means the device has an active connection to the peer with
the given `IP` address and synchronization can proceed. This can be matched
with the noted values from the step #4.

If there are fewer active connection than expected (e.g. if *D<sub>k</sub>*
does not list `IP` of *D<sub>l</sub>* in the `Connected peers` table), this may
be due to the application postponing the lookup for later, or due to a
restrictive firewall. To ensure it's not the former, give it a time of up to 10
minutes for the devices to find each ohter.

#### Add/Modify/View repository content

In this step we'll add new files to the repository and view it on another device with which
we have an active connection.

The adding itself can be done prior to having any active connection, but the synchronization
will - of course - happen only after the fact.

To add a file to the repository:

* Click on the blue `+` button located in the bottom right corner and then
  click `Add file`.
* Choose a file to add to the repository (prefer images of size ~300kB as larger files
  will make testing slower).
* All other devices with which the current device has active connection should
  start syncing shortly.

