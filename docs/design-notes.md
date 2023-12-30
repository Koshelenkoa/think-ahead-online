# Server - Client
## Event overview 
1. game instance with board and score in the db retrived with code from url 
2. load game with board, score, turn, state (is game going or nah)
3. on user input send request to server with said move 
4. server emit event of board and score changing into room (socket.io has rooms with multiple clients)
5. partial change in dom
## Events
### Client
new-game\
enter-room(room-code)\
set-username\
move\
leave

### Server
```
on.new-game:
  create game entry in db
  send game
on.enter-room:
  send url
on.set-username:
  send callback 200
on.move:
  change-board
on.leave:
  query db set another user win
  to (room, skip=sid) send win
change-board:
  query change to db
  emit
```
  

## Database
mySQL database 
schema outline: 
```
url VARCHAR(20) PRIMARYKEY,
room VARCHAR(5),
board JSON,
player1-sid VARCHAR(10),
player2-sid VARCHAR(10),
player1-nick VARCHAR(25),
player2-nick VARCHAR(25),
score-p1 SMALLINT,
score p2 SMALLINT,
turn TINYINT,
state TINYINT
```
turn - 1st or 2nd player turn\
board - <span style='color: red;'>type is yet to decide</span>\
nicks - look into sanitazation in mySQL as nicks are user-input strings\
player-sid - session ids of socket.io connections\
state:\
  | 0 | game ongoing |
  | 1 | player 1 won |
  | 2 | player 2 won |
