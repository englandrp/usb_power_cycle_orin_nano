### Tool to perform a USB Reset on Realtek Hub 

#### Platforms 
* Tested on NVIDIA Jetson Orin Nano.

* **Note:**  
  It might work on other platforms that have an internal USB Realtek Hub.  
  You can type `lsusb` to check that a hub appears:  

  **Examples:**  
  - On **Nano DevKit**, it should output: `0x0bda::5411`  
  - On **Xavier NX**, it should output: `0x0bda::5489`  
  - On **Orin Nano**, it should output: `0x0bda::5489`  

  Change the `unsigned pid = XXXXX;` in `realtek_hub_power_cycle.c` accordingly.

---

#### Requirements  
* Install **libusb**:  
  ```bash
  sudo apt install libusb-1.0-0-dev

##### How to build : 
Use the build script to build the executable :

`$ sh build.sh`

##### How to use
Once build, execute :

`sudo ./power_cycle`

##### Setting Up Auto-Execution on Boot

`sudo cp power_cycle /usr/local/bin/`

`sudo nano /etc/systemd/system/usb-power-cycle.service`

Add the following content:

```
[Unit]
Description=USB Power Cycle for Realtek Hub
After=multi-user.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/power_cycle
RemainAfterExit=true

[Install]
WantedBy=multi-user.target
```

Enable and start the service:
```
sudo systemctl daemon-reload
sudo systemctl enable usb-power-cycle.service
sudo systemctl start usb-power-cycle.service
```

Now, the USB reset will be performed automatically on every reboot.
