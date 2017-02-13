# fluent-plugin-woothee

'fluent-plugin-woothee' is a Fluentd filter plugin to parse UserAgent strings and to filter/drop specified categories of user terminals (like 'pc', 'smartphone' and so on).

'woothee' is multi-language user-agent strings parser project. See: https://github.com/woothee/woothee

## Configuration

To add woothee parser result into messages:

    <label @accesslog>
      <filter input.**>
        @type woothee
        key_name agent
        merge_agent_info yes
      </filter>
      <match ...>
      </match>
    </label>

Result messages has attributes like 'agent\_name', 'agent\_category' and 'agent\_os' from woothee parser result. If you want to change attribute names, or want to merge more attributes of browser vendor and its version, write configurations as below:

    <label @accesslog>
      <filter input.**>
        @type woothee
        key_name agent
        merge_agent_info yes
        
        out_key_name ua_name
        out_key_category ua_category
        out_key_os ua_os
        out_key_os_version ua_os_version
        out_key_version ua_version
        out_key_vendor ua_vendor
      </filter>
      <match ...>
      </match>
    </label>

To pass messages only with specified user-agent categories (and merge woothee parser result), configure like this:

    <label @accesslog>
      <filter input.**>
        @type woothee
        key_name agent
        merge_agent_info yes
        filter_categories pc,smartphone,mobilephone,appliance
      </filter> # logs of other categories will be dropped
      
      # ...
    </label>

Or, you can specify categories to drop (and not to merge woothee result):

    <label @accesslog>
      <filter input.**>
        @type woothee
        key_name agent
        merge_agent_info false # default
        drop_categories crawler
      </filter>
      
      # ...
    </label>

### Fast Crawler Filter

If you want to drop __almost__ all of messages with crawler's user-agent, and not to merge woothee result, you just specify plugin type:

    <filter input.**>
      @type woothee_fast_crawler_filter
      key_name useragent
    </filter>

'fluent-plugin-woothee' uses 'Woothee.is\_crawler' of woothee with this configuration, fast and incomplete method to judge user-agent is crawler or not.
If you want to drop all of crawlers completely, specify 'type woothee' and 'drop_categories crawler'.

### Output plugin

The output version of woothee plugin is not supported in versions for Fluentd v0.14.

## TODO

* patches welcome!

## Copyright

* Copyright (c) 2012- TAGOMORI Satoshi (tagomoris)
* License
  * Apache License, Version 2.0
