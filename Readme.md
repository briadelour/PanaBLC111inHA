#Panasonic BL-111 configuration in Home Assistant
Breathe new lift into an old network camera from ~2008.  

## Steps to add camera to HA
- Add MJPEG IP Camera using MJPEG URL: http://192.168.xx.yy/nphMotionJpeg?Resolution=320x240&Quality=Clarity
- Enter the username and password

## Add PTZ controls to HA for camera
- In Configuration.yaml
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
- **NOTE:** Change the camadmin to your username, change 123456 to your passowrd, and set the correct private IP for your network of the camera.

## Example Card with PTZ controls for camera
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
          service: rest_command.garage_cam_homeposition
          data:
            action: home
            entity_id: camera.garage_cam
```
