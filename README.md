# WordGame

Word Game for DTML

Create a word matching game with Phaser 3(using JavaScript), in which the player can match vocabs in other languages with their english equivalents.

 ## An example would be something like this:

![alt text](https://github.com/jackzhang12/WordGame/blob/master/WordMatchingGame.jpg)

 ## A typical flow of DTML game looks like this:
![alt text](https://github.com/jackzhang12/WordGame/blob/master/GameFlow.jpg)

## API Calls

### There are three API calls which are required for any DTML game:

Record Game start event.  This call record user starting the game:

```
 export default class extends Phaser.State {
 ...
 init() {
	var data = { "envelop": null, 
	             "page": "<YOUR GAME NAME>", 
	             "time": null, 
	              "eventType": "GameStarted", 
	              "eventData": navigator.userAgent 
	            };
	
        fetch('https://dtml.org/Activity/Record/', 
		{ method: 'post',
		  credentials: 'same-origin', 
		  body: JSON.stringify(data),
		  headers: {
      			   'content-type': 'application/json'
    			   }
		}).catch(err => {
                console.log('err', err)
            });
    }
    ...
    }
```
Record User Progress (multiple calls)
```
recordUserProgress(word, isCorrect) {
        var url = 'https://dtml.org/api/GameService/Reinforcement?value=';
        var type = isCorrect ? "correct_word" : "wrong_word";
        fetch(url + word+'&type='+type +'&source=<YOUR GAME NAME>', 
        {
			credentials: 'same-origin', 
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            }
        })
            .then(res => res.json())
            .then(data => {
                console.log(data);
            })
            .catch(err => {
                console.log('err', err)
            });
    }
 ```
Record Game End event
```
  callGameOverService(score, complexity) {
        var url = ''https://dtml.org/Activity/RecordUserActivity?id=';
        fetch(url + '<YOUR GAME NAME>'+'&score=' + score + '&complexity=' + complexity, 
        {
			credentials: 'same-origin', 
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            }
        })
            .then(res => res.json())
            .then(data => {
                console.log(data);
            })
            .catch(err => {
                console.log('err', err)
            });
    }
```
