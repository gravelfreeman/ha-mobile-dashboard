# graphite-dashboard-mobile
Sharing my custom dashboard since my friend asked for it. Since he offered me to help with the code I thought it would make sense to host it on my Github. Might as well share it publicly so that more people can help.

The goal of this mobile dashboard is to fix 2 things that I think are missing from Home Assistant. First looking good. Second being actually practical. Home-Assistant is a very good tool but I think it's ugly and all over the place. This dashboard isn't ready all of the box. You'll still have the pleasure to setup and learn Home-Assistant to your liking. It's more like a way to organize your dashbaord to achieve a clean look while keeping it practical.

## 3. Rooms

Here I'm using [UI-Lovelace-Minimalist room cards](https://ui-lovelace-minimalist.github.io/UI/usage/cards/card_room/) but without UI-Lovelace-Minimalist which I find it to be too much overhead and I like to keep my install as minimal as possible. I'm using an [Horizontal Stack Card](https://www.home-assistant.io/dashboards/horizontal-stack/) in order to show 2 room card per row which I found to be the best for a mobile dashboard. Those room cards are the only way to navigate the rooms in the dashboard which leave the dashboard menu to be used for other stuff. The room card will display:

- Room name and icon
- Up to 4 customizable icons (ex. climate, lights, appliance)
- Room temperature and humidity
- Customizable color

I like to use the first icon for the climate control and the second icon for lights control. Basically those icons are a shortcut to avoid navigating in the room and control that room faster. I reserve the third and fourth icons in case that a room has an appliance I'd like to quickly access. Examples would be media center in living room, oven in kitchen, laundry appliances in bathroom, etc.

### 3.1 Add the card-room template

In order to be able to use the room cards in your dashboard you'll have to edit your dashboard in the Raw Configuration Editor. Copy the code from [room-card.yaml](https://github.com/gravelfreeman/ha-mobile-dashboard/blob/main/rooms.yaml
) into your dashboard raw yaml configuration file.

You must paste it before the data of your configuration file. It should be before your first page and exactly before:

`title: YOUR_TITLE`

### 3.2 Add a custom card-room in your dashboard

The way it work is that you creating a custom template card for 2 rooms that'll be diplayed horizontally. Add as many as you want and they'll nicely work together. Here's an example for 2 rooms.

```
type: vertical-stack
title: Pièces
cards:
  - type: horizontal-stack
    cards:
      - type: custom:button-card
        template:
          - card_room
          - purple_no_state # THIS VALUE CHANGE THE COLOR OF YOUR CARD
        name: ROOM_NAME
        icon: mdi:ROOM_ICON
        entity: sensor.THERMOSTAT_ROOM_NAME
        tap_action:
          action: navigate
          navigation_path: /lovelace/ROOM_PATH
        variables:
          label_use_temperature: true
          label_use_brightness: false
          humidity: sensor.HUMIDITY_ROOM_NAME
          entity_1: # THIS EXAMPLE IS FOR CONTROLLING CLIMATE IN THAT ROOM
            entity_id: climate.THERMOSTAT_ROOM_NAME
            templates:
              - climate # THIS VALUE CHANGE THE COLOR OF YOUR BUTTON DEPENDING ON STATE, MUST BE SETUP IN ROOM-CARD.YAML
            tap_action:
              action: more-info
            hold_action:
              action: none
            double_tap_action:
              action: none
            icon: mdi:ICON_NAME
          entity_2: # THIS EXAMPLE IS FOR TURNING OFF ALL LIGHTS IN THAT ROOM
            entity_id: sensor.LIGHTS_ROOM_NAME # SEE 1.2.2 IN THIS GUIDE TO CREATE THIS HELPER
            templates:
              - light
            tap_action:
              action: call-service
              service: light.toggle
              service_data:
                area_id:
                  - ROOM_NAME
            hold_action:
              action: none
            double_tap_action:
              action: none
          entity_3: # THIS EXAMPLE IS FOR A CONFIGURED APPLE TV IN HA
            entity_id: media_player.REPLACETHIS
            templates:
              - media_player # THIS VALUE CHANGE THE COLOR OF YOUR BUTTON DEPENDING ON STATE, MUST BE SETUP IN ROOM-CARD.YAML
            tap_action:
              action: navigate
              navigation_path: /lovelace/ROOM_PATH
            hold_action:
              action: call-service
              service: remote.send_command
              service_data:
                device_id: REPLACETHIS
                command:
                  - suspend
                  - wakeup
              icon: mdi:television # EXAMPLE FOR MEDIA CENTER
            double_tap_action:
              action: none
      - type: custom:button-card
        template:
          - card_room
          - purple_no_state # THIS VALUE CHANGE THE COLOR OF YOUR CARD
        name: ROOM_NAME
        icon: mdi:ROOM_ICON
        entity: sensor.THERMOSTAT_ROOM_NAME
        tap_action:
          action: navigate
          navigation_path: /lovelace/ROOM_PATH
        variables:
          label_use_temperature: true
          label_use_brightness: false
          humidity: sensor.HUMIDITY_ROOM_NAME
          entity_1: # THIS EXAMPLE IS FOR CONTROLLING CLIMATE IN THAT ROOM
            entity_id: climate.THERMOSTAT_ROOM_NAME
            templates:
              - climate # THIS VALUE CHANGE THE COLOR OF YOUR BUTTON DEPENDING ON STATE, MUST BE SETUP IN ROOM-CARD.YAML
            tap_action:
              action: more-info
            hold_action:
              action: none
            double_tap_action:
              action: none
            icon: mdi:ICON_NAME
          entity_2: # THIS EXAMPLE IS FOR TURNING OFF ALL LIGHTS IN THAT ROOM
            entity_id: sensor.LIGHTS_ROOM_NAME # SEE 1.2.2 IN THIS GUIDE TO CREATE THIS HELPER
            templates:
              - light
            tap_action:
              action: call-service
              service: light.toggle
              service_data:
                area_id:
                  - ROOM_NAME
            hold_action:
              action: none
            double_tap_action:
              action: none
          entity_3: # THIS EXAMPLE IS FOR A CONFIGURED APPLE TV IN HA
            entity_id: media_player.REPLACETHIS
            templates:
              - media_player # THIS VALUE CHANGE THE COLOR OF YOUR BUTTON DEPENDING ON STATE, MUST BE SETUP IN ROOM-CARD.YAML
            tap_action:
              action: navigate
              navigation_path: /lovelace/ROOM_PATH
            hold_action:
              action: call-service
              service: remote.send_command
              service_data:
                device_id: REPLACETHIS
                command:
                  - suspend
                  - wakeup
              icon: mdi:television # EXAMPLE FOR MEDIA CENTER
            double_tap_action:
              action: none
```
