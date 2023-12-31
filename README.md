# mysql2
A modified gem for Debian 12 using mysql2 gem version 0.3.x to bypass error below:

 `Incorrect MySQL client library version! This gem was compiled for x.x.x-MariaDB but the client library is x.x.x.`

## Source
|GitHub|Branch|
|---|----|
|https://github.com/brianmario/mysql2|0.3.x|

## Installation
There's 2 way to install this gem:
  1. Install gem through GitHub
     
      Add this following code in Gemfile
     
      ```RUBY
      gem 'mysql2', git: 'https://github.com/Techno-Indonesia/techno-gems.git', branch: 'debian-12/mysql2/0.3.21'
      ```
  2. Install gem locally
      
      ```
      git clone -b debian-12/mysql2/0.3.21 https://github.com/Techno-Indonesia/techno-gems.git
      cd mysql2
      gem build mysql2.gemspec
      gem install ./mysql2-0.3.21.gem
      ```
        
      
## What's modified?
1. lib/mysql2/client.rb
   
    <sub>from:</sub>
    ```RUBY
    12|  :connect_flags => REMEMBER_OPTIONS | LONG_PASSWORD | LONG_FLAG | TRANSACTIONS | PROTOCOL_41 | SECURE_CONNECTION,
    ```

    <sub>to:</sub>
    ```RUBY
    12|  :connect_flags => REMEMBER_OPTIONS | LONG_FLAG | TRANSACTIONS | PROTOCOL_41 | SECURE_CONNECTION,
    ```

2. ext/mysql2/client.c

    <sub>from:</sub>
    ```C
    1119|  for (i = 0; lib[i] != 0 && MYSQL_LINK_VERSION[i] != 0; i++) {
    1120|    if (lib[i] == '.') {
    1121|      dots++;
    1122|              /* we only compare MAJOR and MINOR */
    1123|      if (dots == 2) break;
    1124|    }
    1125|    if (lib[i] != MYSQL_LINK_VERSION[i]) {
    1126|      rb_raise(rb_eRuntimeError, "Incorrect MySQL client library version! This gem was compiled for %s but the client library is %s.", MYSQL_LINK_VERSION, lib);
    1127|      return;
    1128|    }
    1129|  }
    ```

    <sub>to:</sub>
    ```C
    1119|  // for (i = 0; lib[i] != 0 && MYSQL_LINK_VERSION[i] != 0; i++) {
    1120|  //   if (lib[i] == '.') {
    1121|  //     dots++;
    1122|  //             /* we only compare MAJOR and MINOR */
    1123|  //     if (dots == 2) break;
    1124|  //   }
    1125|  //   if (lib[i] != MYSQL_LINK_VERSION[i]) {
    1126|  //     rb_raise(rb_eRuntimeError, "Incorrect MySQL client library version! This gem was compiled for %s but the client library is %s.", MYSQL_LINK_VERSION, lib);
    1127|  //     return;
    1128|  //   }
    1129|  // }
    ```