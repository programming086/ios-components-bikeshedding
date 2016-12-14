# ios-components-bikeshedding
often we create same components again and again. this is short reminder that simple feature contains really a lot of pieces

--------------

## Chat

### Chat UI 
You can select from different libs, with or without bubbles:

  * https://github.com/jessesquires/JSQMessagesViewController (huge library with many customization options, but release 7 is real mess inside. controller incapsulates datasource, delegate and view logic)
  * https://github.com/slackhq/SlackTextViewController
  * https://github.com/LoganWright/SimpleChat

### Basic actions

As user i want to:

  * send text message (image, my current location, video)
  * receive message
  * read history (older messages)
  * fetch new messages (newer that user has; user may miss these messages on laggy connection)

### Receiving messages
  
  * manually reload chat to fetch new messages
  * fetch new messages automatically
    * use HTTP polling (easy way) 
    * use web sockets
  * show that user is typing message 
  * scroll to new message

### Sending messages
  
  * show typing indicator for one user when another starts typing
  * send message

### Message status

  * message is being typed
  * message was sent
  * message was delivered
  * message was read/open
  * message failed to send (read below)

### Connection issues

  * message sending error: 
    * indicate that message sending failed (show warning button/icon, change message background)
    * tap on message -> show retry button/action sheet
    * re-send message
    * put message in correct place (handle sending order)
  
  * message receiving error (no connection or server error):
    * show 'establishing connection' indicator
    * fetch new messages when connection was established

### Message order
  
User should see messages in order as they were send. Racecondition could appear due to connection lags. Set sequence number of timestamp on client side to sort messages in correct order. 

### Empty view

Show cute 'no messages' view to push user to start chatting.
Lib https://github.com/dzenbot/DZNEmptyDataSet

### Notifications
  
 * send push notification when new message is received
 * receive push notification and open correspondent chat window
 * allow to hide content/author of message (a-la Messages)
 * virbate/play audio when message is received/sent
  

### Unread
 
 * show number of unread messages
 * indicate that chat has unread messages in list of chats
 * show 'unread' messages inside chat
 * mark message as read

### Transport layer

You can send messages via http to your server or user sockets/mqtt.

 * ios side:
   * https://github.com/square/SocketRocket
   * https://github.com/jmesnil/MQTTKit
 * server side + ios lib:
   * https://pusher.com/
   * https://www.pubnub.com/
   * https://github.com/RocketChat/Rocket.Chat

### Security

Use https and encrypt messages using private keys.

Encryption libs:

  * https://github.com/cossacklabs/themis/
  * https://github.com/RNCryptor/RNCryptor
  * https://github.com/aerogear/aerogear-crypto-ios


### Application life cycle

Active chat always shows actual data: last chat message, message counter, chat name, user's name.

Ensure handling correctly such cases:

 * chat is opened > device looses connection > device restores connection
 * chat is opened > connection type changes from wi-fi to cellular and vice versa
 * chat is opened > device screen is turned off manually > user activates device and app opens
 * chat is opened > device goes to sleep > user activates device and app opens
 * chat is opened > app goes to background > app goes to foreground

### Media

  * show placeholder while media data is downloading
  * allow to cancel sending message (stop upload media file)
  * save photos/videos to camera roll

### Advanced
  
 actions:
  * allow to edit text message
  * allow to delete message (text, media). show delete animation
  * share message (open native sharing action sheet)
  
 logic:
  * store local history (when user opens chat, show cached messages)
  * reload chat in background to show always latest data
  * allow to rename chat
  * open user profile on tapping on his avatar
 
 ui:
  * custom keyboard/stickers
  * message bubbles dynamic behaviour (if any)
  * data types support: urls, tel, dates (tap on link - open webpage (in Safari, in inner webbrowser))


--------------

## Table / List

### UI

 * cells with static height
 * cells with dynamic height
 * footer/header/sections

### Basic actions

 * smooth scrolling
 * tap on cell to perform action (open new screen)
 * pull-down-to reload (pull from top to reload whole list)
 * pull-down-to fetch new data (Twitter style)
 * load more (pull from bottom)

### More actions

 * tap on button inside cell (like/reply buttons)
 * swipe to left to open cell menu (archive, remove, edit, more)
 * remove cell (swipe to left) with animation
 * add cell with animation
 * reload single cell

### States

 * table no data:
  * show fullscreen 'no data' view
  * table is loading, show full screen 'loading' view 
  * connection is broken, show full screen 'error tap to retry' view
  
 * table has data (easy mode):
  * table is loading, show refresh indicator
  * connection is broken, show error indicator
 
 
### Media

 * use placeholders
 * 'smart' download:
  * start download images on current and next visible cells
  * increase download queue priority for current visible cells
  * decrease priority for far invisible cells
 * fade-in image when it's downloaded

### Advanced

--------------

## Comments

### Basic actions

As user i want to:

* add comment to object (post, image, video, etc.)
* fetch new comments
* read old comments
* delete my comment
* flag inappropriate comment

### Receiving comments

* manually reload list of comments to get new comments
* automatically load new comments when screen with comments is opened

Usually comments should be displayed in a sequence they were sent. Set timestamp on client side when adding new comment.

### Adding comments

* show characters limit, if we have one
* send comment: show new comment immediately and send it in background, or wait for successful response from server
* show error in case comment wasn't added due to connectivity / server issues
* store comment failed to send locally and add ability to resend it

### Deleting comments:
* use simple interaction recognized by user to delete comment (swipe, single tap)
* show confirmation dialog before deleting comment
* show error in case comment wasn't deleted due to connectivity / server issues

### Flagging comments:
* use simple interaction recognized by user to flag comment (swipe, single tap)
* show confirmation dialog before flagging comment
* show error in case comment wasn't flagged due to connectivity / server issues

Error handling should depend on how you show activity to your users - blocking interaction and showing error message on same screen, or sending request in background so user should see error message even when navigating to other screens.

### States

* there are no comments
* comments are loading
* comments couldn't be loaded
* comments are reloading / failed to reload
* load more comments / failed to load more

### Advanced

* Nested comments:
 * comments may have several levels
 * clearly indicate on which level user is posting his comment (reply to post, reply to other comment)
 
## Apple Pay
 
### Testing
 
1. Use device with latest iOS version
2. Create test account in itunesconnect for your app, enable Apple Pay for this account
3. Change phone region to USA
4. Logout form App Store and iCloud
5. Login into iCloud using test account
6. Open Wallet and add test card from the list (https://developer.apple.com/support/apple-pay-sandbox/)
* ???
7. Profit
