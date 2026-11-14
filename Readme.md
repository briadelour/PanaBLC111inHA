# Panasonic BL-C111A configuration in Home Assistant
[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-donate-yellow?logo=buymeacoffee&logoColor=white)](https://buymeacoffee.com/briadelour)
[![PayPal](https://img.shields.io/badge/PayPal-donate-blue?logo=paypal&logoColor=white)](https://www.paypal.com/paypalme/bdeloureiro)

Breathe new life into an old network camera from ~2008.  
<img width="498" height="284" alt="image" src="https://github.com/user-attachments/assets/672c34c6-69ec-4c57-b95a-40e1b44e7fbe" />

## Prerequisites

- Home Assistant instance with access to `configuration.yaml`
- Panasonic BL-C111A camera on your local network
- Camera's IP address (e.g., `192.168.xx.yy`)
- Camera admin credentials

## Setup Instructions

### Step 1: Add Camera to Home Assistant

1. In Home Assistant, add a new **MJPEG IP Camera** integration
2. Use the following MJPEG URL (replace `192.168.xx.yy` with your camera's IP):
   ```
   http://192.168.xx.yy/nphMotionJpeg?Resolution=320x240&Quality=Clarity
   ```
3. Enter your camera's username and password when prompted

### Step 2: Configure PTZ Controls

Add the following REST commands to your `configuration.yaml` file:

```yaml
rest_command:
  garage_cam_scan:
    url: "http://camadmin:123456@192.168.xx.yy/nphControlCamera?Direction=PanScan&Resolution=640x480&Quality=Clarity&Mode=MPEG-4&RPeriod=0&Size=STD&PresetOperation=Move&Language=0"
  garage_cam_panleft:
    url: "http://camadmin:123456@192.168.xx.yy/nphControlCamera?Direction=PanLeft&Resolution=640x480&Quality=Clarity&Mode=MPEG-4&RPeriod=0&Size=STD&PresetOperation=Move&Language=0"
  garage_cam_panright:
    url: "http://camadmin:123456@192.168.xx.yy/nphControlCamera?Direction=PanRight&Resolution=640x480&Quality=Clarity&Mode=MPEG-4&RPeriod=0&Size=STD&PresetOperation=Move&Language=0"
  garage_cam_tiltup:
    url: "http://camadmin:123456@192.168.xx.yy/nphControlCamera?Direction=TiltUp&Resolution=640x480&Quality=Clarity&Mode=MPEG-4&RPeriod=0&Size=STD&PresetOperation=Move&Language=0"
  garage_cam_tiltdown:
    url: "http://camadmin:123456@192.168.xx.yy/nphControlCamera?Direction=TiltDown&Resolution=640x480&Quality=Clarity&Mode=MPEG-4&RPeriod=0&Size=STD&PresetOperation=Move&Language=0"
  garage_cam_home:
    url: "http://camadmin:123456@192.168.xx.yy/nphControlCamera?Direction=HomePosition&Resolution=640x480&Quality=Clarity&Mode=MPEG-4&RPeriod=0&Size=STD&PresetOperation=Move&Language=0"
```

**⚠️ Important:** Replace the following values:
- `camadmin` → your camera's username
- `123456` → your camera's password
- `192.168.xx.yy` → your camera's IP address
- `garage_cam_*` → customize service names for your setup

### Step 3: Restart Home Assistant

Restart Home Assistant to load the new REST commands.

## Dashboard Card with PTZ Controls

Add this card to your Lovelace dashboard for interactive camera control:

```yaml
type: vertical-stack
cards:
  - camera_view: live
    fit_mode: cover
    type: picture-glance
    title: Garage PTZ
    camera_image: camera.garage_cam
    entities:
      - entity: camera.garage_cam
      - entity: camera.garage_cam
        icon: mdi:arrow-left
        tap_action:
          action: call-service
          service: rest_command.garage_cam_panleft
          data:
            action: left
            entity_id: camera.garage_cam
      - entity: camera.garage_cam
        icon: mdi:arrow-right
        tap_action:
          action: call-service
          service: rest_command.garage_cam_panright
          data:
            action: right
            entity_id: camera.garage_cam
      - entity: camera.garage_cam
        icon: mdi:arrow-up
        tap_action:
          action: call-service
          service: rest_command.garage_cam_tiltup
          data:
            action: up
            entity_id: camera.garage_cam
      - entity: camera.garage_cam
        icon: mdi:arrow-down
        tap_action:
          action: call-service
          service: rest_command.garage_cam_tiltdown
          data:
            action: down
            entity_id: camera.garage_cam
      - entity: camera.garage_cam
        icon: mdi:arrow-collapse-horizontal
        tap_action:
          action: call-service
          service: rest_command.garage_cam_scan
          data:
            action: scan
            entity_id: camera.garage_cam
      - entity: camera.garage_cam
        icon: mdi:home
        tap_action:
          action: call-service
          service: rest_command.garage_cam_home
          data:
            action: home
            entity_id: camera.garage_cam
```

**Note:** Make sure to update `camera.garage_cam` to match your camera's entity ID.

## PTZ Control Functions

The configuration provides the following camera controls:
- **Pan Left/Right** - Horizontal camera movement
- **Tilt Up/Down** - Vertical camera movement
- **Scan** - Automated pan scan mode
- **Home** - Return to home position

## Troubleshooting

- If PTZ controls don't work, verify your camera's IP address is correct and reachable
- Check that credentials in `configuration.yaml` match your camera's admin account
- Ensure Home Assistant has been restarted after configuration changes
- Test REST commands individually using Home Assistant's Developer Tools

## Security Note

⚠️ **Warning:** Credentials in `configuration.yaml` are stored in plain text. Ensure your Home Assistant instance is properly secured and not exposed to the internet without proper authentication.

## License

This configuration is provided as-is for personal use.
