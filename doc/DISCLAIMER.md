## List of known public services

* Ask on one of the following rooms: #mautrix_yunohost:matrix.fdn.fr or #telegram:maunium.net

## Bridging usage
** Note that several Telegram and Matrix users can be bridged, each Telegram account has its own bot administration room. If they are in a same Telegram group, only one matrix room will be created. **

### Bridge a Telegram user and a Matrix user
* First your Matrix user or Synapse Server has to be authorized in the Configuration of the bridge (see below)
* Then, invite the bot (default @telegrambot:yoursynapse.domain) in this new Mautrix-Telegram bot administration room.
  * If the Bot does bot accept, see the [troubleshooting page](https://docs.mau.fi/bridges/general/troubleshooting.html)
* Send ``!tg help`` to the bot in the created room to know how to control the bot.
See also [upstream wiki Authentication page](https://docs.mau.fi/bridges/python/telegram/authentication.html)

#### Logging into Telegram account
* Type ``login`` or ``login-qr`` (untested) in main management room (`!tg` in any bridge room)
* follow setup instructions from bot
* By defaults, only conversations with very recent messages and groups will be suggested to be bridged
* Accept invitations to the bridged chat rooms

#### Registering the Bridge as a primary device (untested)
* Type ``!tg register <phone>``, where ``<phone>`` is your phone number in the internation format with no space, e.g. ``!tg register +33612345678``
* Answer in the bot room with the verification code that you reveived in SMS.
* Set a profile name with ``!tg set-profile-name <name>``

### Double puppeting
* Log in with ``login-matrix <access token>``
* After logging in, the default Matrix puppet of your Telegram account should leave rooms and your account should join all rooms the puppet was in automatically.


### Relaybot: Bridge a group for several Matrix and several Telegram users to chat together
* see https://docs.mau.fi/bridges/python/telegram/relay-bot.html

## Configuration of the bridge

The bridge is [roughly configured at installation](https://github.com/YunoHost-Apps/mautrix_telegram_ynh/blob/master/conf/config.yaml), e.g. allowed admin and user of the bot. Finer configuration can be done by modifying the
following configuration file with SSH: 
```/opt/yunohost/mautrix_telegram/config.yaml```
and then restarting the mautrix_telegram service.

## Documentation

 * Official "Mautrix-Telegram" documentation: https://docs.mau.fi/bridges/python/telegram/index.html
 * Matrix room (Matrix Bridges in Yunohost): #mautrix_yunohost:matrix.fdn.fr
 * Matrix room (upstream app): #telegram:maunium.net
In case you need to upload your logs somewhere, be aware that they contain your contacts' and your phone numbers. Strip them out with 
``| sed -r 's/[0-9]{10,}/📞/g' ``
 * "Mautrix-Telegram" bridge is based on the [telegram daemon](https://gitlab.com/telegramd/telegramd) project.
 * YunoHost documentation: If more specific documentation is needed, feel free to contribute.

## YunoHost specific features

#### Multi-user support

* Bot users are not related to Yunohost users. Any Matrix account or Synapse server autorized in the configuration of the bridge can invite/use the bot. 
* The Telegram bot is a local Matrix-Synapse user, but accessible through federation (synapse public or private).
* Several Telegram and Matrix users can be bridged with one bridge, each user has its own bot administration room. 
* If several bot users are in a same Telegram group, only one Matrix room will be created by the bridge.
* See https://github.com/YunoHost-Apps/synapse_ynh#multi-users-support

#### Multi-instance support

* Multi-instance installation should work. Several bridge instances could be installed for one Matrix-Synapse instance so that one Matrix user can bridge several Telegram accounts. 
* Several bridge instances could be installed for each Matrix-Synapse instance to benefit from it. But one bridge can be used by users from several Matrix-Synapse instances.

## Troubleshooting

* Check the spelling of the bot name!
* For slow servers it might be necessary to run ``systemctl edit mautrix_telegram.service`` (also for `coturn.service` and `coturn-synapse.service`) and insert
```
[Service]
ExecStartPre=/bin/sleep 90
```
such that it is ensured that synapse is running before the bridge tries to connect.
(If it worked after installation but broke after a restart this probably is it.)
