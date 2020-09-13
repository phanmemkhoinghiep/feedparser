# sensor.feedparser
RSS feed custom component for [Home Assistant](https://www.home-assistant.io/) which can be used in conjunction with the custom [Lovelace](https://www.home-assistant.io/lovelace) [list-card](https://github.com/custom-cards/list-card)

[![GitHub Release][releases-shield]][releases]
[![License][license-shield]](LICENSE.md)

![Project Maintenance][maintenance-shield]
[![GitHub Activity][commits-shield]][commits]

[![Discord][discord-shield]][discord]
[![Community Forum][forum-shield]][forum]


To get started put `/custom_components/feedparser/` here:
`<config directory>/custom_components/feedparser/`

**Example configuration.yaml:**

```yaml

#Feedreader

sensor:
  platform: feedparser
  name: Tin tức RSS
  feed_url: 'https://vnexpress.net/rss/tin-moi-nhat.rss'
  
  automation:
    
  - id: 'thong_bao_tin_tuc_cap_nhat'
    alias: "thong_bao_tin_tuc_cap_nhat"
    trigger:
      platform: state
      entity_id: 
      - sensor.tintuc_rss      
    condition:
      condition: and
      conditions:
        - condition: time
          after: '13:30'
          before: '12:00'
        - condition: template
          value_template: "{{ trigger.to_state.state != trigger.from_state.state }}"      
    action:
      service: script.doc_tin    
      
script:
  doc_tin:
    sequence:
    - service: media_player.volume_set
      data:
        entity_id: media_player.vali_demo
        volume_level: '0.5'
    - service: tts_viettel.say      
      data_template:
        message: "Có tin mới buổi {{ states('sensor.sys_buoi') }} lúc {{ as_timestamp(now()) | timestamp_custom('%I:%M:%S %p, %d/%m/%Y', true) }} như sau: {{states.sensor.tin_tuc_rss.attributes.entries}} " 
        entity_id: media_player.vali_demo
        speed: '1'
        voice_type: '{{["nu_mien_bac_01" , "nu_mien_bac_02" , "nam_mien_bac_01" , "nam_mien_bac_02" , "nu_mien_trung_01", "nam_mien_trung_01" , "nu_mien_nam_01" , "nu_mien_nam_02" , "nu_mien_nam_03" , "nam_mien_nam_01"] |random }} '  
       
          

Note: Will return all fields if no inclusions or exclusions are specified

Due to how `custom_components` are loaded, it is normal to see a `ModuleNotFoundError` error on first boot after adding this, to resolve it, restart Home-Assistant.

[commits-shield]: https://img.shields.io/github/commit-activity/y/custom-components/feedparser.svg?style=for-the-badge
[commits]: https://github.com/custom-components/feedparser/commits/master
[discord]: https://discord.gg/Qa5fW2R
[discord-shield]: https://img.shields.io/discord/330944238910963714.svg?style=for-the-badge
[forum-shield]: https://img.shields.io/badge/community-forum-brightgreen.svg?style=for-the-badge
[forum]: https://community.home-assistant.io/t/custom-component-rss-feed-parser/64637
[license-shield]: https://img.shields.io/github/license/custom-components/feedparser.svg?style=for-the-badge
[maintenance-shield]: https://img.shields.io/badge/maintainer-Ian%20Richardson%20%40iantrich-blue.svg?style=for-the-badge
[releases-shield]: https://img.shields.io/github/release/custom-components/feedparser.svg?style=for-the-badge
[releases]: https://github.com/custom-components/feedparser/releases
