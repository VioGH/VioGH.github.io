# VioGH.github.io
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>СигмаКомбат</title>
</head>
<body>
    <H1>СигмаКомбат</H1>
    <form name="search">
        <input type="text" name="key" placeholder="Введите имя" />
        <input type="button" name="print" value="Кликнуть" />
        <input type="button" name="printG" value="Получить клики" />
    </form>
    <div id="printBlock"></div>
    <script>
        //document.write("<h2>Простой пример работы Get pапроса</h2>");
        const keyBox = document.search.key;
        //const keyText = document.search.keyT;
        const sendBtn = document.search.print;
        const getBtn = document.search.printG;
        //const keyItemBox = document.search.keyItem;
        var clicks = 0;
 
        // обработчик изменения текста
        function onchange(e){
            // получаем элемент printBlock
            const printBlock = document.getElementById("printBlock");
            // получаем новое значение
            const val = e.target.value;
            // установка значения
            printBlock.textContent = val;
        }
        // обработка потери фокуса
        function onblur(e){
     
            // получаем его значение и обрезаем все пробелы
            const text = keyBox.value.trim();
            if(text==="")
                keyBox.style.borderColor = "red";
            else
                keyBox.style.borderColor = "green";
        }
        // получение фокуса
        function onfocus(e){
     
            // установка цвета границ поля
            keyBox.style.borderColor = "blue";
        }
        function PutZapros()
        {
            const url = keyBox.value.trim();
            const keyVal = keyBox.value.trim();

            clicks = clicks + 1;
            printBlock.textContent = "Кликов: " + clicks;
            
            
            //let num = Number(keyVal);
            fetch("https://eiri4839e9difj339-default-rtdb.europe-west1.firebasedatabase.app/Users/" + url +".json", {
                method: 'PUT',
                body: JSON.stringify({
                    tokens: clicks //nameBox.value.trim()
            }),
            headers: {
                'Content-Type': 'application/json',
            },
            })
            .then((res) => res.json())
            .then((data) => console.log(data))
            .catch((err) => console.error(err));
        }
        //https://eiri4839e9difj339-default-rtdb.europe-west1.firebasedatabase.app/Users/.json
        function GetZapros()
        {
            const url = keyBox.value.trim();
            const keyVal = keyBox.value.trim();

            fetch("https://eiri4839e9difj339-default-rtdb.europe-west1.firebasedatabase.app/Users/" + url +"/tokens.json")
            .then(response => response.text())
            //.then(responseText => console.log(responseText));
            .then(responseText => clicks = Number(responseText))
            .then(responseText => printBlock.textContent = "Кликов: " + Number(responseText));
            //PutZapros();
        }
        
        //keyBox.addEventListener("change", onchange);
        keyBox.addEventListener("blur", onblur);
        keyBox.addEventListener("focus", onfocus);
        getBtn.addEventListener("click", GetZapros);
        sendBtn.addEventListener("click", PutZapros);
    </script>
</body>
</html>
