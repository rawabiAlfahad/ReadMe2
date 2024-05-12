# ReadMe2
## JavaScript button syncs two pages and changes color button on two pages:

page one :

```
<!-- Page 1 -->
<!DOCTYPE html>
<html lang="en">
<head> 
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page 1</title>
</head>
<body>
    <button id="syncButton1">Available</button>
    <button id="syncButton2">Available</button>
    <button id="syncButton3">Available</button>
	<button id="syncButton4">Available</button>
	

    <script>
        const channel = new BroadcastChannel('page_sync_channel');
        const syncButtons = [
            document.getElementById('syncButton1'),
            document.getElementById('syncButton2'),
            document.getElementById('syncButton3'),
			document.getElementById('syncButton4')
        ];
        
        const updateButtonState = (syncButton) => {
            const savedState = localStorage.getItem(syncButton.id);
            if (savedState) {
                const { text, color } = JSON.parse(savedState);
                syncButton.innerText = text;
                syncButton.style.backgroundColor = color;
            }
        };

        syncButtons.forEach(button => {
            updateButtonState(button);

            button.addEventListener('click', () => {
                let newState;
                if (button.innerText === 'Available') {
                    newState = {
                        text: 'Not Available',
                        color: 'red'
                    };
                } else {
                    newState = {
                        text: 'Available',
                        color: 'green'
                    };
                }
                button.innerText = newState.text;
                button.style.backgroundColor = newState.color;
                localStorage.setItem(button.id, JSON.stringify(newState)); 
                channel.postMessage({ id: button.id, ...newState });
            });
        });
        
        channel.addEventListener('message', (event) => {
            const { id, text, color } = event.data;
            const syncButton = document.getElementById(id);
            syncButton.innerText = text;
            syncButton.style.backgroundColor = color;
            localStorage.setItem(id, JSON.stringify({ text, color })); 
        });
    </script>

</body>
</html>
```

page two:
```
<!-- Page 2 -->
<!DOCTYPE html>
<html  lang="en">
<head> 
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page 1</title>
</head>
<body>
    <button id="syncButton1">Available</button>
    <button id="syncButton2">Available</button>
    <button id="syncButton3">Available</button>
	 <button id="syncButton4">Available</button> <!-- if you have more button add in two pages -->

    <script>
        const channel = new BroadcastChannel('page_sync_channel');
        const syncButtons = [
            document.getElementById('syncButton1'),
            document.getElementById('syncButton2'),
            document.getElementById('syncButton3'),
			 document.getElementById('syncButton4')       <!-- and her -->
        ]; 
        
        const updateButtonState = (syncButton) => {
            const savedState = localStorage.getItem(syncButton.id);
            if (savedState) {
                const { text, color } = JSON.parse(savedState);
                syncButton.innerText = text;
                syncButton.style.backgroundColor = color;
            }
        };

        syncButtons.forEach(button => {
            updateButtonState(button);

            button.addEventListener('click', () => {
                let newState;
                if (button.innerText === 'Available') {
                    newState = {
                        text: 'Not Available',
                        color: 'red'
                    };
                } else {
                    newState = {
                        text: 'Available',
                        color: 'green'
                    };
                }
                button.innerText = newState.text;
                button.style.backgroundColor = newState.color;
                localStorage.setItem(button.id, JSON.stringify(newState)); 
                channel.postMessage({ id: button.id, ...newState });
            });
        });
        
        channel.addEventListener('message', (event) => {
            const { id, text, color } = event.data;
            const syncButton = document.getElementById(id);
            syncButton.innerText = text;
            syncButton.style.backgroundColor = color;
            localStorage.setItem(id, JSON.stringify({ text, color })); 
        });
    </script>

</body>
</html>
```
