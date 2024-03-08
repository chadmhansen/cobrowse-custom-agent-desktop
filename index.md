<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Custom Desktop with Co-browse</title>
    <style>
        iframe {
            width: 900px;
            height: 600px;
        }
    </style>
</head>
<body>
    <div>
        <input type="text" placeholder="conversationId" id="conversationId" />
        <select name="lang" id="lang">
            <option value="ar">ar</option>
            <option value="cs">cs</option> 
            <option value="da">da</option> 
            <option value="de">de</option>
            <option value="en-us">en-us</option>
            <option value="es">es</option>
            <option value="fi">fi</option> 
            <option value="fr">fr</option>
            <option value="he">he</option>
            <option value="it">it</option>
            <option value="ja">ja</option> 
            <option value="ko">ko</option>
            <option value="nl">nl</option>
            <option value="no">no</option>
            <option value="pl">pl</option>
            <option value="pt-br">pt-br</option>
            <option value="pt-pt">pt-pt</option>
            <option value="ru">ru</option>
            <option value="sv">sv</option>
            <option value="tr">tr</option>
            <option value="zh-cn">zh-cn</option>
        </select>
        <button id="startCobrowse">start Co-browse</button>
    </div>

    <script>
        document.getElementById('startCobrowse').addEventListener('click', () => {
            const conversationId = document.getElementById('conversationId').value;
            const lang = document.getElementById('lang').value;
            startCobrowse(conversationId, lang);
        });

        function startCobrowse(conversationId, lang) {
            console.log(conversationId, lang);

            const iframe = document.createElement('iframe');
            iframe.setAttribute('id', 'cobrowseFrame');

            const url = new URL('https://apps.mypurecloud.com/cobrowse-next/viewer.html');
            url.searchParams.append('conversationId', conversationId);
            url.searchParams.append('lang', lang);

            iframe.setAttribute('src', url.href);
            document.body.append(iframe);
        }


        // Hide or remove the iframe when Co-browse session ends
        window.addEventListener('message', (event) => {
            // In a real application, be sure to check the event origin: 
            // https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage#security_concerns
            if (event.data) {

                if (event.data.action && event.data.action === 'closeScreenShare') {
                    console.log('co-browse ended, removing the iframe');
                    document.getElementById('cobrowseFrame').remove();
                }

    
                if (event.data.joinCode && event.data.sessionId) {
                    const joinCode = event.data.joinCode;
                    const sessionId = event.data.sessionId;
                }      
            }
        });

    </script>
</body>
</html>
