# Battlesnake CLI

This tool allows running a Battlesnake game locally. There are several command-
line options, including the ability to send snakes requests sequentially or all
at the same time, and also to set a timeout limit.

I do plan to package this as a PR onto the [BattlesnakeOfficial/rules](https://github.com/BattlesnakeOfficial/rules) 
repo eventually, after some polishing.

## To-Do List

I don't expect this repo to last long enough for the Issue workflow to be worth
using, so I'll provide here some short-term plans and things I'm thinking of:

* ~~Sending the /end request to Snakes at the end of the game~~
* ~~A flag to set request timeout values~~
* ~~Making the request-sending parallel by default, to speed up execution~~
* ~~A flag to un-parallel request-sending, to better enable debugging~~
* ~~A flag to use different game types (royale, squad, solo, etc)~~
* ~~Various levels of verbosity (eg- quiet, errors-only, states, or full-maps)~~
* Integrating the Cobra toolkit for CLI commands
* Integrating the whole kit-and-kaboodle into the official rules Repo

## Usage

### Build It
```
go build
```

### Run It
```
Usage:
  battlesnake-cli [OPTIONS]

Application Options:
  -w, --width=      Width of Board
  -h, --height=     Height of Board
  -n, --name=       Name of Snake
  -u, --url=        URL of Snake
  -S, --squad=      Squad of Snake
  -t, --timeout=    Request Timeout
  -s, --sequential  Use Sequential Processing
  -g, --gametype=   Type of Game Rules
  -v, --viewmap     View the Map Each Turn

Help Options:
  -h, --help        Show this help message
```

Names and URLs will be paired together in sequence, so in the following example
it effectively makes:

* Snake1: http://snake1-url-whatever:port
* Snake2: http://snake2-url-whatever:port

Names are optional, but definitely way easier to read than UUIDs. URLs are
optional too, but your snake will lose if the server is only sending move
requests to http://example.com.


```
./battlesnake-cli --width 7 --height 7 --name Snake1 --url http://snake1-url-whatever:port --name Snake2 --url http://snake2-url-whatever:port
```

### Sample Output
```
$ ./battlesnake-cli --width 3 --height 3 --url http://redacted:4567/ --url http://redacted:4568/  --name Bob --name Sue
2020/10/31 22:05:56 [1]: State: &{3 3 [{1 0}] [{e74892ba-9f0c-4e96-9bde-1a9efaff0ab4 [{0 1} {0 2} {0 2} {0 2}] 100  } {89e20d26-7da7-4964-b0ae-148c8f60f7ee [{2 1} {2 2} {2 2} {2 2}] 100  }]} OutOfBounds: []
2020/10/31 22:05:56 [2]: State: &{3 3 [{1 0}] [{e74892ba-9f0c-4e96-9bde-1a9efaff0ab4 [{0 0} {0 1} {0 2} {0 2}] 99  } {89e20d26-7da7-4964-b0ae-148c8f60f7ee [{2 0} {2 1} {2 2} {2 2}] 99  }]} OutOfBounds: []
2020/10/31 22:05:56 [3]: State: &{3 3 [{1 2}] [{e74892ba-9f0c-4e96-9bde-1a9efaff0ab4 [{1 0} {0 0} {0 1} {0 2} {0 2}] 100 head-collision 89e20d26-7da7-4964-b0ae-148c8f60f7ee} {89e20d26-7da7-4964-b0ae-148c8f60f7ee [{1 0} {2 0} {2 1} {2 2} {2 2}] 100 head-collision e74892ba-9f0c-4e96-9bde-1a9efaff0ab4}]} OutOfBounds: []
2020/10/31 22:05:56 [DONE]: Game completed after 3 turns. It was a draw.
```

### Sample Map Output
```
$ ./battlesnake-cli --url http://redacted:4567/ --url http://redacted:4567/ --url http://redacted:4567/ --url http://redacted:4567/ --url http://redacted:4567/ --url http://redacted:4567/ --url http://redacted:4567/ --url http://redacted:4567/ --name Snake1 --name Snake2 --name Snake3 --name Snake4 --name Snake5 --name Snake6 --name Snake7 --name Snake8 --width 13 --height 13 --timeout 1000 --viewmap
2020/11/01 21:43:03 [2]
Hazards ░: []
Food ⚕: [{12 10} {10 10} {9 6} {9 12}]
Snake1 ⌘: {5730794b-0748-4202-a124-82433cf0012f [{0 4} {0 3} {0 2}] 98  }
Snake2 ⌀: {dce2c8b0-d97d-4453-a334-db608361442e [{8 2} {8 1} {8 0} {8 0}] 100  }
Snake3 ●: {c115d57e-2534-4815-aa48-cabc986c3109 [{9 11} {8 11} {7 11} {7 11}] 100  }
Snake4 ⍟: {e0b5f430-96f7-466c-a78d-419e8377693c [{3 3} {3 2} {3 1}] 98  }
Snake5 ◘: {9e3e4525-393a-4905-83dd-0b0b5007f2f6 [{5 3} {5 2} {5 1}] 98  }
Snake6 ☺: {9c1a4ff7-b5d8-4f90-89f7-82f6ca52c652 [{8 4} {7 4} {6 4} {6 4}] 100  }
Snake7 ◉: {6b81369e-4b2d-4c70-89eb-608db43befe0 [{1 11} {1 12} {2 12} {2 12}] 100  }
Snake8 ◍: {6cb5a2a6-d974-40b7-a616-5a0d817a8a9e [{5 7} {4 7} {4 6}] 98  }
◦◉◉◦◦◦◦◦◦⚕◦◦◦
◦◉◦◦◦◦◦●●●◦◦◦
◦◦◦◦◦◦◦◦◦◦⚕◦⚕
◦◦◦◦◦◦◦◦◦◦◦◦◦
◦◦◦◦◦◦◦◦◦◦◦◦◦
◦◦◦◦◍◍◦◦◦◦◦◦◦
◦◦◦◦◍◦◦◦◦⚕◦◦◦
◦◦◦◦◦◦◦◦◦◦◦◦◦
⌘◦◦◦◦◦☺☺☺◦◦◦◦
⌘◦◦⍟◦◘◦◦◦◦◦◦◦
⌘◦◦⍟◦◘◦◦⌀◦◦◦◦
◦◦◦⍟◦◘◦◦⌀◦◦◦◦
◦◦◦◦◦◦◦◦⌀◦◦◦◦
```

### Sample Solo Game
```
$ ./battlesnake-cli --url http://redacted:4567/ --name Bob --width 3 --height 3 --timeout 500 --gametype solo
2020/10/31 22:02:58 [1]: State: &{3 3 [{2 2}] [{cc8831e8-d517-4216-a8d8-a64243decada [{1 2} {0 2} {0 2}] 99  }]} OutOfBounds: []
2020/10/31 22:02:58 [2]: State: &{3 3 [{2 1}] [{cc8831e8-d517-4216-a8d8-a64243decada [{2 2} {1 2} {0 2} {0 2}] 100  }]} OutOfBounds: []
2020/10/31 22:02:59 [3]: State: &{3 3 [{0 1}] [{cc8831e8-d517-4216-a8d8-a64243decada [{2 1} {2 2} {1 2} {0 2} {0 2}] 100  }]} OutOfBounds: []
2020/10/31 22:02:59 [4]: State: &{3 3 [{0 1}] [{cc8831e8-d517-4216-a8d8-a64243decada [{1 1} {2 1} {2 2} {1 2} {0 2}] 99  }]} OutOfBounds: []
2020/10/31 22:02:59 [5]: State: &{3 3 [{0 2}] [{cc8831e8-d517-4216-a8d8-a64243decada [{0 1} {1 1} {2 1} {2 2} {1 2} {1 2}] 100  }]} OutOfBounds: []
2020/10/31 22:02:59 [6]: State: &{3 3 [{2 0}] [{cc8831e8-d517-4216-a8d8-a64243decada [{0 2} {0 1} {1 1} {2 1} {2 2} {1 2} {1 2}] 100  }]} OutOfBounds: []
2020/10/31 22:02:59 [7]: State: &{3 3 [{2 0} {0 0}] [{cc8831e8-d517-4216-a8d8-a64243decada [{0 1} {0 2} {0 1} {1 1} {2 1} {2 2} {1 2}] 99 snake-self-collision cc8831e8-d517-4216-a8d8-a64243decada}]} OutOfBounds: []
2020/10/31 22:02:59 [DONE]: Game completed after 7 turns. It was a draw.
```

### Sample Squad Game
```
-$ ./battlesnake-cli --url http://redacted:4567/ --name Bob --squad A --url http://redacted:4567/ --name Sue --squad A --url http://redacted:4567/ --name Jim --squad B --url http://redacted:4567/ --name Francine --squad B --width 5 --height 5 --gametype squad
2020/10/31 22:14:27 [1]: State: &{5 5 [{2 4} {4 1} {4 3} {1 4} {0 2}] [{92a1bd60-8f8d-4adb-8468-e8eb1028b7f0 [{3 0} {4 0} {4 0}] 99  } {25c5607c-a2da-421e-84c3-e2a040cffae5 [{1 2} {1 1} {1 1}] 99  } {9dc22d73-3631-43cc-9472-a2ff074bc4a1 [{3 2} {4 2} {4 2}] 99  } {54157a58-2e07-4f84-b035-6d6df73d751a [{3 4} {4 4} {4 4}] 99  }]} OutOfBounds: []
2020/10/31 22:14:28 [2]: State: &{5 5 [{4 1} {4 3} {1 4}] [{92a1bd60-8f8d-4adb-8468-e8eb1028b7f0 [{2 0} {3 0} {4 0} {4 0}] 100  } {25c5607c-a2da-421e-84c3-e2a040cffae5 [{0 2} {1 2} {1 1} {1 1}] 100  } {9dc22d73-3631-43cc-9472-a2ff074bc4a1 [{3 3} {3 2} {4 2} {4 2}] 100  } {54157a58-2e07-4f84-b035-6d6df73d751a [{2 4} {3 4} {4 4} {4 4}] 100  }]} OutOfBounds: []
2020/10/31 22:14:28 [3]: State: &{5 5 [{4 1}] [{92a1bd60-8f8d-4adb-8468-e8eb1028b7f0 [{2 1} {2 0} {3 0} {4 0}] 99  } {25c5607c-a2da-421e-84c3-e2a040cffae5 [{0 3} {0 2} {1 2} {1 1}] 99  } {9dc22d73-3631-43cc-9472-a2ff074bc4a1 [{4 3} {3 3} {3 2} {4 2} {4 2}] 100  } {54157a58-2e07-4f84-b035-6d6df73d751a [{1 4} {2 4} {3 4} {4 4} {4 4}] 100  }]} OutOfBounds: []
2020/10/31 22:14:28 [4]: State: &{5 5 [{4 1}] [{92a1bd60-8f8d-4adb-8468-e8eb1028b7f0 [{3 1} {2 1} {2 0} {3 0}] 98  } {25c5607c-a2da-421e-84c3-e2a040cffae5 [{0 4} {0 3} {0 2} {1 2}] 98  } {9dc22d73-3631-43cc-9472-a2ff074bc4a1 [{4 4} {4 3} {3 3} {3 2} {4 2}] 99  } {54157a58-2e07-4f84-b035-6d6df73d751a [{1 3} {1 4} {2 4} {3 4} {4 4}] 99  }]} OutOfBounds: []
2020/10/31 22:14:28 [5]: State: &{5 5 [{1 0}] [{92a1bd60-8f8d-4adb-8468-e8eb1028b7f0 [{4 1} {3 1} {2 1} {2 0} {2 0}] 100 squad-eliminated } {25c5607c-a2da-421e-84c3-e2a040cffae5 [{0 3} {0 4} {0 3} {0 2}] 97 snake-self-collision 25c5607c-a2da-421e-84c3-e2a040cffae5} {9dc22d73-3631-43cc-9472-a2ff074bc4a1 [{3 4} {4 4} {4 3} {3 3} {3 2}] 98  } {54157a58-2e07-4f84-b035-6d6df73d751a [{2 3} {1 3} {1 4} {2 4} {3 4}] 98  }]} OutOfBounds: []
2020/10/31 22:14:28 [DONE]: Game completed after 5 turns. Francine is the winner.
```

### Sample Royale Game
```
$ ./battlesnake-cli --url http://redacted:4567/ --url http://redacted:4567/ --name Bob --name Sue --width 7 --height 7 --timeout 800 --gametype royale
2020/10/31 22:16:44 [1]: State: &{7 7 [{4 0} {0 0} {3 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{4 1} {5 1} {5 1}] 99  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{0 1} {1 1} {1 1}] 99  }]} OutOfBounds: []
2020/10/31 22:16:44 [2]: State: &{7 7 [{3 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{4 0} {4 1} {5 1} {5 1}] 100  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{0 0} {0 1} {1 1} {1 1}] 100  }]} OutOfBounds: []
2020/10/31 22:16:45 [3]: State: &{7 7 [{3 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 0} {4 0} {4 1} {5 1}] 99  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{1 0} {0 0} {0 1} {1 1}] 99  }]} OutOfBounds: []
2020/10/31 22:16:45 [4]: State: &{7 7 [{3 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 1} {3 0} {4 0} {4 1}] 98  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{1 1} {1 0} {0 0} {0 1}] 98  }]} OutOfBounds: []
2020/10/31 22:16:45 [5]: State: &{7 7 [{3 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 2} {3 1} {3 0} {4 0}] 97  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{1 2} {1 1} {1 0} {0 0}] 97  }]} OutOfBounds: []
2020/10/31 22:16:45 [6]: State: &{7 7 [{0 4}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 3} {3 2} {3 1} {3 0} {3 0}] 100  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{1 3} {1 2} {1 1} {1 0}] 96  }]} OutOfBounds: []
2020/10/31 22:16:45 [7]: State: &{7 7 [{0 4}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{2 3} {3 3} {3 2} {3 1} {3 0}] 99  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{1 4} {1 3} {1 2} {1 1}] 95  }]} OutOfBounds: []
2020/10/31 22:16:45 [8]: State: &{7 7 [{1 1}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{2 4} {2 3} {3 3} {3 2} {3 1}] 98  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{0 4} {1 4} {1 3} {1 2} {1 2}] 100  }]} OutOfBounds: []
2020/10/31 22:16:45 [9]: State: &{7 7 [{1 1}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{2 5} {2 4} {2 3} {3 3} {3 2}] 97  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{0 3} {0 4} {1 4} {1 3} {1 2}] 99  }]} OutOfBounds: []
2020/10/31 22:16:45 [10]: State: &{7 7 [{1 1}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 5} {2 5} {2 4} {2 3} {3 3}] 96  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{0 2} {0 3} {0 4} {1 4} {1 3}] 98  }]} OutOfBounds: [{6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:45 [11]: State: &{7 7 [{1 1}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 4} {3 5} {2 5} {2 4} {2 3}] 95  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{1 2} {0 2} {0 3} {0 4} {1 4}] 97  }]} OutOfBounds: [{6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:45 [12]: State: &{7 7 [{1 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 3} {3 4} {3 5} {2 5} {2 4}] 94  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{1 1} {1 2} {0 2} {0 3} {0 4} {0 4}] 100  }]} OutOfBounds: [{6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:46 [13]: State: &{7 7 [{1 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{2 3} {3 3} {3 4} {3 5} {2 5}] 93  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{2 1} {1 1} {1 2} {0 2} {0 3} {0 4}] 99  }]} OutOfBounds: [{6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:46 [14]: State: &{7 7 [{2 0}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{1 3} {2 3} {3 3} {3 4} {3 5} {3 5}] 100  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{3 1} {2 1} {1 1} {1 2} {0 2} {0 3}] 98  }]} OutOfBounds: [{6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:46 [15]: State: &{7 7 [{2 0}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{1 4} {1 3} {2 3} {3 3} {3 4} {3 5}] 99  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{3 0} {3 1} {2 1} {1 1} {1 2} {0 2}] 97  }]} OutOfBounds: [{6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:46 [16]: State: &{7 7 [{2 0}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{2 4} {1 4} {1 3} {2 3} {3 3} {3 4}] 98  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{4 0} {3 0} {3 1} {2 1} {1 1} {1 2}] 96  }]} OutOfBounds: [{6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:46 [17]: State: &{7 7 [{2 0}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 4} {2 4} {1 4} {1 3} {2 3} {3 3}] 97  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{4 1} {4 0} {3 0} {3 1} {2 1} {1 1}] 95  }]} OutOfBounds: [{6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:46 [18]: State: &{7 7 [{2 0}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 3} {3 4} {2 4} {1 4} {1 3} {2 3}] 96  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{4 2} {4 1} {4 0} {3 0} {3 1} {2 1}] 94  }]} OutOfBounds: [{6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:46 [19]: State: &{7 7 [{2 0}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{2 3} {3 3} {3 4} {2 4} {1 4} {1 3}] 95  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{5 2} {4 2} {4 1} {4 0} {3 0} {3 1}] 93  }]} OutOfBounds: [{6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:46 [20]: State: &{7 7 [{2 0}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{2 2} {2 3} {3 3} {3 4} {2 4} {1 4}] 94  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{5 1} {5 2} {4 2} {4 1} {4 0} {3 0}] 92  }]} OutOfBounds: [{0 0} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:46 [21]: State: &{7 7 [{2 0}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{2 1} {2 2} {2 3} {3 3} {3 4} {2 4}] 93  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{5 0} {5 1} {5 2} {4 2} {4 1} {4 0}] 90  }]} OutOfBounds: [{0 0} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:46 [22]: State: &{7 7 [{4 4}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{2 0} {2 1} {2 2} {2 3} {3 3} {3 4} {3 4}] 99  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{6 0} {5 0} {5 1} {5 2} {4 2} {4 1}] 88  }]} OutOfBounds: [{0 0} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:47 [23]: State: &{7 7 [{4 4} {4 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 0} {2 0} {2 1} {2 2} {2 3} {3 3} {3 4}] 97  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{6 1} {6 0} {5 0} {5 1} {5 2} {4 2}] 86  }]} OutOfBounds: [{0 0} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:47 [24]: State: &{7 7 [{4 4} {4 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 1} {3 0} {2 0} {2 1} {2 2} {2 3} {3 3}] 96  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{6 2} {6 1} {6 0} {5 0} {5 1} {5 2}] 84  }]} OutOfBounds: [{0 0} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:47 [25]: State: &{7 7 [{4 4} {4 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 2} {3 1} {3 0} {2 0} {2 1} {2 2} {2 3}] 95  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{5 2} {6 2} {6 1} {6 0} {5 0} {5 1}] 83  }]} OutOfBounds: [{0 0} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:47 [26]: State: &{7 7 [{4 4} {4 3}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{3 3} {3 2} {3 1} {3 0} {2 0} {2 1} {2 2}] 94  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{5 1} {5 2} {6 2} {6 1} {6 0} {5 0}] 82  }]} OutOfBounds: [{0 0} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:47 [27]: State: &{7 7 [{4 4}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{4 3} {3 3} {3 2} {3 1} {3 0} {2 0} {2 1} {2 1}] 100  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{4 1} {5 1} {5 2} {6 2} {6 1} {6 0}] 81  }]} OutOfBounds: [{0 0} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:47 [28]: State: &{7 7 [{3 6}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{4 4} {4 3} {3 3} {3 2} {3 1} {3 0} {2 0} {2 1} {2 1}] 100  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{4 0} {4 1} {5 1} {5 2} {6 2} {6 1}] 79  }]} OutOfBounds: [{0 0} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:47 [29]: State: &{7 7 [{3 6}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{5 4} {4 4} {4 3} {3 3} {3 2} {3 1} {3 0} {2 0} {2 1}] 99  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{5 0} {4 0} {4 1} {5 1} {5 2} {6 2}] 77  }]} OutOfBounds: [{0 0} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:48 [30]: State: &{7 7 [{3 6} {4 6}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{5 3} {5 4} {4 4} {4 3} {3 3} {3 2} {3 1} {3 0} {2 0}] 98  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{6 0} {5 0} {4 0} {4 1} {5 1} {5 2}] 75  }]} OutOfBounds: [{0 0} {0 1} {0 2} {0 3} {0 4} {0 5} {0 6} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:48 [31]: State: &{7 7 [{3 6} {4 6}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{5 2} {5 3} {5 4} {4 4} {4 3} {3 3} {3 2} {3 1} {3 0}] 97  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{6 1} {6 0} {5 0} {4 0} {4 1} {5 1}] 73  }]} OutOfBounds: [{0 0} {0 1} {0 2} {0 3} {0 4} {0 5} {0 6} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:48 [32]: State: &{7 7 [{3 6} {4 6}] [{07ba7c7a-6533-4682-8769-fc2666b155c5 [{5 1} {5 2} {5 3} {5 4} {4 4} {4 3} {3 3} {3 2} {3 1}] 96  } {7b33dbd3-c9c5-461c-8d66-29ca715a9e43 [{5 1} {6 1} {6 0} {5 0} {4 0} {4 1}] 72 head-collision 07ba7c7a-6533-4682-8769-fc2666b155c5}]} OutOfBounds: [{0 0} {0 1} {0 2} {0 3} {0 4} {0 5} {0 6} {1 0} {2 0} {3 0} {4 0} {5 0} {6 0} {6 1} {6 2} {6 3} {6 4} {6 5} {6 6}]
2020/10/31 22:16:48 [DONE]: Game completed after 32 turns. Bob is the winner.
```
