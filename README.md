# Home Assistant Australian BoM Radar Loop Creator in Docker
Docker container for Home Assistant BoM Radar Loop plus individual radar images to enable use in LLM Vision

The Home Assitant LLM Vision analyser requires separate radar images to process when looking for storm events.

This Docker container creates those images, plus a useful animated GIF for showing on your dashboards. There is also a useful timestamp file for use with an AI prompt to help it understand the BoM image timestamps.

Note that I run this on a Synology NAS so it is set up for that in terms of directories.

Just change `/volume1/docker/bom_radar_downloader` to the path that suits your file system.

## Installation
Create the following directory structure somewhere on the machine where you run Docker:
```
/volume1/docker/bom_radar_downloader/
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
├── bom_radar_downloader.py
├── config.yaml
├── IDR.legend.0.png
└── images/  (created automatically)
```

Edit the `config.yaml` file to suit your requirements. The intructions are in the file on what to change.

On your Home Assistant create a folder under `/config/www` as per your config file in the `SMB Share Configuration` section.

Then build and run as required by Docker.

## Home Location Marker

You can add a small house icon to the radar loop to mark your home location:

1. In `config.yaml`, set `residential_location.enabled: true`
2. Set your home latitude and longitude
3. The house icon will appear on the animated GIF (not on individual PNG files)

### Troubleshooting Marker Position

If your home marker appears in the wrong location (common with composite/rainbow radars), you can override the radar center coordinates:

```yaml
radar:
  product_id: IDR952
  timezone: Australia/Melbourne
  # Override the radar center if your home marker is misaligned
  latitude: -37.0     # Adjust until marker is correct
  longitude: 143.5    # Adjust until marker is correct
  km_per_pixel: 2.0   # Match your radar range (512km=4.0, 256km=2.0, 128km=1.0, 64km=0.5)
```

**How to find the correct coordinates:**
- If your home appears too far **east**, increase the longitude value
- If your home appears too far **west**, decrease the longitude value
- If your home appears too far **north**, increase the latitude value (less negative)
- If your home appears too far **south**, decrease the latitude value (more negative)
- Each 1 degree ≈ 111km north/south, ≈ 89km east/west (at Melbourne's latitude)
