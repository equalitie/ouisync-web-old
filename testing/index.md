# Testing OuiSync

Please help test OuiSync by following the steps below! Test scripts are listed 
in order of how likely something is to fail (from least to most likely), thus
it's a good idea to start with the first and progress to the next each time the 
test succeeds. Alternatively, one can start with a later test case and then either 
progress or regress depending on whether the current test succeeds or fails.

The following test can be performed either locally (where all the devices are connected
to the same WiFI), globally (where all the devices connect through the internet), or
a mix of the two.

We will begin with one device labeled *C* which will be the one to "create a
repository". Then we'll have other device labeled *O<sub>i</sub>* which will "add" the
created repository. Once the shared repository is added to the other device(s),
the distinction between who is the creator and who is not will disappear and all 
devices will sync a single repository, *D<sub>i</sub>*.

The steps of the test are as follows:

#### Clean slate

1. Uninstall the OuiSync app if it has been previously installed and reinstall it on all devices.
2. Ensure the `App version` on each device is the same:
   * Open the app
   * Click on the settings icon (gear wheel in the top right corner)
   * Scroll all the way down to see the field `App version`
4. Ensure there are no previously-created repositories (they should have been deleted
   by performing Step #1).
4. Note down the following fields from the Settings page on both devices:
   * `Connection type`
   * `External IP`
   * `Local IPv4` and `Local IPv6`
   * `Listening on QUIC/UDP IPv4` and `Listening on QUIC/UDP IPv6`

#### Create a repository

5. Hit the "back" button to exit the settings and return to the main page on all devices.
6. Click on the "Create a Repository" button on device *C*. A dialog shall then prompt you
   to enter a name and password for this new repository. This password is unique to
   the device and will not be implicitly shared with other devices.

#### Share the repository

7. Still on device *C*, from the main page, click on the Share <i class="fa-solid fa-share-nodes"></i> icon in the top
   right corner.
8. In the `Set permission` field select `Write`. This will allow the other
   device(s) (*O<sub>i</sub>*) to make changes to the repository.
9. Send the `Share link` presented in the other field from device *C* to the
   *O<sub>i</sub>* device(s) by either copy/pasting the link to another app or
   sharing it directly with another app via the Share button.

#### Add the repository

10. On device *O<sub>i</sub>* copy the `Share link` it received and open the OuiSync app.
12. Click the `Add a Shared Repository` button. You will be presented with a dialog.
13. Paste the `Share link` into the first field and give the repository a name and password
    (these do not need to match the name and password from device *C* and may also be different
    on all *O<sub>i</sub>* devices).

Device *C* and all devices on which the `Share link` was added (*O<sub>i</sub>*) are 
said to "share the same repository". The distinction between which device "created" the repository 
and which device "added" the repository is no longer important. Thus we'll refer to these devices as just *D<sub>i</sub>*.

#### Check connectivity

13. Go to the setting page and from the click on the button in the field `Connected peers`.

You'll be presented with a table of peers OuiSync knows about. In this table,
the two important columns are the `IP` and the `State`. If the `State` is set
to `Active`, that means the device has an active connection to the peer with
the given `IP` address and synchronization can proceed. This can be matched
with the noted values from Step #4.

If there are fewer active connections than expected (e.g. if *D<sub>k</sub>*
does not list `IP` of *D<sub>l</sub>* in the `Connected peers` table), this may
be due to the application postponing the lookup for later, or due to a
restrictive firewall. To ensure it's not the former, please wait up to 10
minutes to allow the devices to find each other.

#### Add/Modify/View repository content

In this step we'll add a new file to the repository and view it on another device with which
we have an active connection.

Adding a file can be done prior to having any active connection, but synchronization on the other 
device(s) will, of course, happen only after a connection has been made.

To add a file to the repository:

* Click on the blue `+` button located in the bottom right corner and then
  click `Add file`.
* Choose a file to add to the repository (prefer images under ~300kB as larger files
  will make testing slower).
* All other devices with which the current device has an active connection should
  start syncing shortly.
