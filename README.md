//For my host
const http = require("http");
const express = require("express");
const app = express();

app.get("/", (request, response) => {
  console.log(Date.now() + " Ping Received");
  response.sendStatus(200);
});
app.listen(process.env.PORT);

setInterval(() => {
  http.get(`http://${process.env.PROJECT_DOMAIN}.glitch.me/`);
}, 280000);

//Starting creting index.js
const Discord = require("discord.js");
var mysql = require("mysql");
var colldown = new Set();
//Creating my pool connection
var con = mysql.createPool({
  host: "PODAM NA 1000 SUBÓW NA KANALE STUDZIAK :P",
  user: "PODAM NA 1000 SUBÓW NA KANALE STUDZIAK :P",
  password: "PODAM NA 1000 SUBÓW NA KANALE STUDZIAK :P",
  database: "PODAM NA 1000 SUBÓW NA KANALE STUDZIAK :P",
  debug: false,
  charset: "utf8mb4",
  port: 3306,
  connectTimeout: 60 * 60 * 10000
});
//Stary kod
// con.connect(err => {
// if(err) throw err
// console.log('Połączono z bazą danych')
// })

//Variables
const prefix = "&";
let channel_2;
let author;
let authorID;
let authorImg;
let channelID;

// Create a new Discord client
const client = new Discord.Client();
//Creting my help embed
const exampleEmbed = new Discord.MessageEmbed()
  .setTitle("<:MarketingLogo:688694803420282890> __**MARKETING**__")
  .addField(":dividers: Wersja bota:", "2.3.5", true)
  .addField(
    ":abacus: Serwer support",
    "[Kliknij żeby dołączyć](https://discord.gg/6F4KSDk)",
    true
  )
  .addField(
    ":robot: Link do zaproszenia bota",
    "[Kliknij żeby dodać](https://discordapp.com/oauth2/authorize?client_id=688312861927276554&permissions=2147483127&scope=bot)",
    false
  )
  .addField(":brain: Developerzy bota", "Mondonno#6652 | dsonyy#1895", true)
  .addField(
    ":pushpin: Dodatkowe informacje",
    "Marketing to bot reklamowy nowej generacji, z różnymi dodatkowymi funkcjami i komendami moderacyjnymi! Jeśli szukasz bota który zjednoczy inne boty reklamowe, dorze trafiłeś! Dołącz do [serwera support](https://discord.gg/6F4KSDk) aby dowiedzieć się więcej!"
  )
  .setFooter("© by Mondonno#6652 2020");

// when the client is ready, run this code
// this event will only trigger one time after logging in

var message = "";
//Declaring my "on" message event
client.on("message", async message => {
  //Old Code
  //if(!message.guild.me.hasPermission("ADMINISTRATOR")) return message.channel.send("> ⛔️ Bot musi miec permisje `ADMINISTRATOR` abyś mógł wypisywać komendy")

  //Mention Detection
  if (message.mentions.has(client.user) && !message.mentions.everyone) {
    //Creting my mention detection embed
    const embed = new Discord.MessageEmbed()
      .setTitle("Co się stało?")
      .setThumbnail(
        "https://cdn.discordapp.com/attachments/688391374252670986/712209657875398706/sleep.png"
      )
      .setDescription(
        "**Aby uzyskać pomoc w sprawie bot'a napisz `&pomoc`\nJeśli masz problem z konfiguracją napisz `&jak`**"
      )
      .setFooter(
        "Komenda została wywołana przez: " +
          message.author.tag +
          " | ID: " +
          message.author.id,
        message.author.avatarURL()
      )
      .setTimestamp();
    //Sending reply
    return message.reply(embed);
  }
  if (!message.content.startsWith(prefix) || message.author.bot) return;
  // if(message.content.startsWith(prefix)&& message.guild.id != 688006607858434061) return message.channel.send("> ⚙️ Naprawiamy błąd bota \n\n> 🔴 **Status:** Włączony, wyłączone komendy. Naprawa błędu")
  const args = message.content
    .slice(prefix.length)
    .trim()
    .split(/ +/g);

  let args2 = args.slice(1).join(" ");
  const command = args.shift().toLowerCase();

  var today = new Date();
  var dd = String(today.getDate()).padStart(2, "0");
  var mm = String(today.getMonth() + 1).padStart(2, "0"); //January is 0!
  var yyyy = today.getFullYear();

  today = mm + "-" + dd + "-" + yyyy;

  var reklama;
  function SendAd() {
    var sql = `Select * from Reklamy`;
    console.log(`${message.guild.id}, ${args}, ${today}`);
    con.query(sql, function(err, result) {
      if (err) throw err;

      if (result < 1) return;
      console.log("Sended");

      client.channels.cache.get(channel_2).send(result.Reklama);
      con.end();
    });
  }

  function Insert() {
    var sqlI = `Insert into Reklamy (IDDS, Reklama, DataDodania) values ('${message.guild.id}', '${args2}', '${today}')`;
    var sql = `SELECT * from Reklamy where IDDS = ${message.guild.id}`;
    //console.log(`${message.guild.id}, ${args}, ${today}`)
    con.query(sql, function(err, row) {
      if (err) throw err;

      return;
    });
    con.query(
      `Insert into Reklamy (IDDS, Reklama, DataDodania, Zatwierdz) values (?, ?, ?, ?)`,
      [message.guild.id, args2, today, "2"],
      function(err) {
        if (err) {
          con.query(
            `UPDATE Reklamy SET Reklama = ? where IDDS = ?`,
            [args2, message.guild.id],
            function(err) {
              con.query(
                `UPDATE Reklamy SET Zatwierdz = ? where IDDS = ?`,
                ["2", message.guild.id],
                function(err) {}
              );
              if (err) throw err;
              console.log(`Updated ${message.guild.name} Ad!`);
            }
          );
          return;
        }
      }
    );

    message.react("688694803420282890");
    message.channel.send("> 📯 **Ustawiono reklamę!**");
  }
  var cooldownDelay = 5;

  if (command === "staty") {
    var Reklama_Skf = "";
    var IS_Tr_R_bool = false;
    var ReklamaStatus = "";
    var channel;
    var Ilosc_Reklam = "";
    var wstep = "";
    con.query(
      `SELECT * from Kanaly where IDDS = ${message.guild.id}`,
      async function(err, rows) {
        if (!rows.length) {
          channel = "<a:false:702810788980588654> Brak kanału do reklam";
        } else {
          wstep = "<a:true:702810851995942922> **Kanał reklam ustawiony na**";
          channel = client.channels.cache.get(rows[0].Kanal_ID);
        }

        await con.query(
          `SELECT * from Reklamy where IDDS = ${message.guild.id}`,
          function(err, rows) {
            if (!rows.length) {
              IS_Tr_R_bool = false;
              Reklama_Skf =
                "> <a:false:702810788980588654> *Brak ustawionej reklamy*";
            } else {
              IS_Tr_R_bool = true;
              Reklama_Skf =
                "> <a:true:702810851995942922>  Reklama jest ustawiona";
              Ilosc_Reklam = rows[0].Wyslano_L;
              if (rows[0].Zatwierdz == 0) {
                ReklamaStatus = `> <a:true:702810851995942922> **Reklama jest zatwierdzona, i jest wysyłana w cyklu ** *"RANDEOX V1.2"*`;
              } else if (rows[0].Zatwierdz == 1) {
                ReklamaStatus =
                  "> <a:false:702810788980588654>  **Reklama została odrzucona, ustaw jeszcze raz prawidłową reklamę**";
              } else if (rows[0].Zatwierdz == 2) {
                ReklamaStatus =
                  "> <a:LoadingBar:712727482322780170> **Reklama jest w kolejce do zatwierdzenia**";
              }
            }

            const staty_embed = new Discord.MessageEmbed()
              .setAuthor(
                "Statystyki serwer'a",
                "http://server260631.nazwa.pl/Kolorowy_GIF.gif"
              )
              .setDescription(
                `> ${wstep} *${channel}*\n\n ${Reklama_Skf}\n\n${ReklamaStatus}`
              )
              .setColor("#00FF00")
              .setFooter(
                "Komenda została wywołana przez: " +
                  message.author.tag +
                  " | ID: " +
                  message.author.id,
                message.author.avatarURL()
              );

            message.channel.send(staty_embed);
            cooldownDelay = 5;
          }
        );
      }
    );
  }
  if (command === "r" || command === "reklama") {
    await con.query(
      `SELECT * from Kanaly where IDDS = ${message.guild.id}`,
      function(err, rows) {
        if (err) throw err;
        if (rows.length) {
          if (
            message.author.id == "694238217385279538" ||
            message.author.id == "602849479355269130" ||
            message.author.id == " 637592175659712532" ||
            message.author.id == "700658462656561215"
          ) {
            message.author.send(
              "<a:false:702810788980588654> **Ten serwer został zablokowany!**"
            );
            return;
          }
          if (!args.length) {
            return message.channel.send(
              ":eye: **Prawidłowe użycie komendy:** ``&r/reklama reklama_serwera``"
            );
          }
          if (message.mentions.everyone) {
            return message.channel.send(
              ":eye: **Prosimy o ustawianie reklam bez pingów!**"
            );
          }
          var regex = /discord.gg/;
          
          var str = args2;
          var result = regex.test(str);
          
          console.log(result);
          

          if (!message.member.hasPermission("ADMINISTRATOR")) {
            const Permisje = new Discord.MessageEmbed()
              .setDescription(
                "<a:false:702810788980588654> **Potrzebujesz permisji** `ADMINISTRATOR` **żeby to zrobić!**"
              )
              .setColor("#FF0000");

            return message.channel.send(Permisje);
          }
          if (result == false) {
            const NoLink = new Discord.MessageEmbed()
              .setDescription(
                "<a:false:702810788980588654> **Reklama nie posiada linku do serwera!**"
              )
              .setColor("#FF0000");
            return message.channel.send(NoLink);
          }
          console.log(args2.length);
          if (args2.length >= 45) {
            const SprEmbed = new Discord.MessageEmbed()
              .setTitle("Nowa reklama!")
              .setDescription(
                `\n\`Serwer:\` ${message.guild.name}\n\n\`Autor:\` ${message.member.user} | **${message.member.user.tag}**\n\n\`Reklama:\` ` +
                  " ```" +
                  `${args2}` +
                  "```" +
                  `\n\n\`Komendy:\`\n\n ` +
                  "```" +
                  `z&odrzuc ${message.guild.id} [powod]\nz&ztw ${message.guild.id}` +
                  "```"
              )
              .setColor("#FFFF");
            client.channels.cache.get("714039546739687455").send(SprEmbed);
            Insert();
            cooldownDelay = 20;
          } else {
            // console.log("XD")
            const NoLetter = new Discord.MessageEmbed()
              .setDescription(
                "<a:false:702810788980588654> **Reklama musi mieć co najmniej** __`45`__ **znaków!**"
              )
              .setColor("#FF0000");
            return message.channel.send(NoLetter);
          }
        } else {
          message.channel.send(
            "> <:NotAllowed:680780614219202565>  **Musisz najpierw ustawić kanał do reklam! Zrób to za pomocą komendy** ``&u #wzmianka-kanału-do-reklam``"
          );

          return;
        }
      }
    );
    //Juz niedługo
  }
  if (command === "&boost") {
    //Już niedługo
  }
  if (command === "zapros") {
    author = message.author.tag;
    authorID = message.author.id;
    const embed = new Discord.MessageEmbed()
      .setTitle(":link:  Kliknij żeby mnie dodać!")
      .setURL(
        "https://discordapp.com/oauth2/authorize?client_id=688312861927276554&permissions=8&scope=bot"
      )
      .setColor("#009eff")
      .setFooter(
        "Komenda została wywołana przez: " + author + " | ID: " + authorID,
        message.author.avatarURL()
      );
    message.channel.send(embed);
    cooldownDelay = 5;
  }
  if (command == "wyswietl") {
    var IFItIS = "";
    await con.query(
      `SELECT * from Reklamy where IDDS = ${message.guild.id}`,
      function(err, rows) {
        if (!rows.length) {
          const wyw = new Discord.MessageEmbed()
            .setDescription(
              "<a:false:702810788980588654> **Brak ustawionej reklamy**"
            )
            .setColor("#FF0000");

          message.channel.send(wyw);
        } else {
          const wyw = new Discord.MessageEmbed()
            .setTitle(`__*ZAWARTOŚĆ REKLAMY TEGO SERWERA*__ `)
            // .setAuthor(
            //   `ZAWARTOŚĆ REKLAMY TEGO SERWERA`,
            //   "https://cdn.discordapp.com/attachments/688391374252670986/718548216647254146/staty-google.gif"
            // )
            .setDescription(` \`\`\`${rows[0].Reklama}\`\`\` `)
            .setColor("#00FF00");
          message.channel.send(wyw);
          cooldownDelay = 5;
        }
      }
    );
  }

  if (command === "jak") {
    const embed = new Discord.MessageEmbed()
      .setTitle(":question: Jak skonfigurować bota?")
      .setThumbnail(
        "https://cdn.discordapp.com/attachments/688391374252670986/705065795213983844/jak.png"
      )
      .setDescription(
        "*Nasz bot jest bardzo prosty w użyciu, więc mozesz go skonfigurować w 3 niezbędnych korkach!*"
      )
      .addField(
        "1️⃣ Krok pierwszy",
        "Pierwszym krokiem jest ustawienie kanału na którym będą wysyłane reklamy innych serwerów. Aby to zrobić musisz napisać na `&u #wzmainka_kanału_do_reklam`"
      )
      .addField(
        "2️⃣ Krok drugi",
        "Napisz `&r treść_reklamy` by ustawić swoją reklamę"
      )
      .addField(
        "3️⃣  Krok trzeci",
        "Wpisz komendę `&staty` aby zobaczyć czy twoja reklama została zaakceptowana/odrzucona/ lub czeka na sprawdzenie"
      )
      .addField(
        "4️⃣ Krok czwarty",
        "W trzecim kroku możesz się cieszyć że pomyślnie skonfigurowałeś bota!\nJeśli chcesz pomóc innym rozpromowac serwery wpisz komendę `&zapros` i roześlij link znajomym!"
      )
      .addField(
        "Masz jakiś problem dot. bot'a?",
        "[Dołącz na serwer support!](https://discord.gg/6F4KSDk)"
      )
      .setColor("#009eff")
      .setFooter(
        "Komenda została wywołana przez: " +
          message.author.tag +
          " | ID: " +
          message.author.id,
        message.author.avatarURL()
      );
    message.channel.send(embed);
    cooldownDelay = 5;
  }
  if (command === "u" || command === "ustaw") {
    if (!args.length) {
      message.channel.send(
        ":eye: **Prawidłowe użycie komendy:** ``&u/ustaw #wzmianka-kanału-do-reklam``"
      );
      return;
    }
    if (!message.member.hasPermission("ADMINISTRATOR")) {
      var Permisje = new Discord.MessageEmbed()
        .setDescription(
          "<a:false:702810788980588654> **Potrzebujesz permisji** `ADMINISTRATOR` **żeby to zrobić!**"
        )
        .setColor("#FF0000");

      return message.channel.send(Permisje);
    }
    author = message.author.tag;
    let channel_l = message.mentions.channels.first();
    authorID = message.author.id;
    const embed = new Discord.MessageEmbed()
      .setTitle(`:bar_chart: **Ustawiono kanał reklam**`)
      .setDescription(`*Ustawiono kanał reklam na* ${channel_l}`)
      .setFooter(
        "Komenda została wywołana przez: " + author + " | ID: " + authorID,
        message.author.avatarURL()
      );
    // con.query(`SELECT * FROM Kanaly where IDDS = ?`,[message.guild.id] ,function(err,rows){
    //    if(!rows.length){
    //    return message.channel.send(':eye: **Prawidłowe użycie komendy:** ``&u/ustaw #wzmianka-kanału-do-reklam``');

    //   }

    // })

    channel_2 = channel_l;
    //  con.query(`Insert into Kanaly (IDDS, Kanal_ID, DataDodania) values (?, ?, ?)`,[message.guild.id, args2, today], function(err){
    //   console.log('hi');
    //   if(err){
    console.log("XXXXX");
    con.query(
      `Insert into Kanaly(IDDS, Kanal_ID, Data_Dodania) values (?, ?, ?)`,
      [message.guild.id, channel_2.id, today],
      function(err) {
        console.log("Run!");
        if (err) {
          con.query(
            `UPDATE Kanaly SET Kanal_ID = ${channel_2.id} where IDDS = ${message.guild.id}`,
            function(err, result) {
              if (err) throw err;
              message.react("688694803420282890");
            }
          );
          return;
        }
      }
    );
    console.log("XXXXX22222");
    channel_l.createInvite({ maxAge: 0 }).then(invite => {
      //  message.channel.send(`***Testowy link do tego serwera*** *#TESTY-NOWYCH-FUNKCJI* https://discord.gg/${invite.code}`)
      con.query(
        `UPDATE Kanaly SET Invite_Code = '${invite.code}' where IDDS = ${message.guild.id}`,
        function(err, result) {
          if (err) throw err;
        }
      );
    });
    message.channel.send(embed);
    cooldownDelay = 15;
  }
  //var botMention = message.content.includes("");
  //if(message.h)
  if (command === "bug") {
    if (!args.length) {
      message.channel.send("> ❌ **Musisz podać treść błędu!**");
      return;
    }
    if (
      message.author.id == "694238217385279538" ||
      message.author.id == "602849479355269130" ||
      message.author.id == "637592175659712532" ||
      message.author.id == "700658462656561215"
    ) {
      message.author.send(
        "<a:false:702810788980588654> **Ten serwer został zablokowany!**"
      );
      return;
    }
    const bug = new Discord.MessageEmbed()
      .setTitle("Zgłoszono błąd do administracji!")
      .setThumbnail(message.author.avatarURL())
      .setDescription(
        "Dziękujemy za zgłąszanie błędu naszego bota do administracji! Jeśli nasi programiści będą potrzebowali więcej informacji dot. błędu skontaktują się z tobą. Jeśli chcesz mieć bezpośredni kontakt z administracją dołącz na [**serwer support**](https://discord.gg/veR9UrF)"
      )
      .addField(
        "Szczegóły błędu",
        `⏰ **Zgłaszający**: ${message.author.tag} \nID: ${message.author.id} \n\n🧭 **Serwer**: ${message.guild.name} \n**ID**: ${message.guild.id} \n\n✏️ **Treść**: ${args2} \n\n 📅 **Data**: ${today}`
      )
      .addField(
        "Wszelkie trollowanie przez tą komendę będzie surowo karane",
        "\u200B"
      )
      .setFooter(
        "Komenda została wywołana przez: " +
          message.author.tag +
          " | ID: " +
          message.author.id,
        message.author.avatarURL()
      );
    const bugADM = new Discord.MessageEmbed()
      .setTitle("Zgłoszono błąd!")
      .setThumbnail(message.author.avatarURL())
      .addField(
        "Szczegóły błędu",
        `⏰ **Zgłaszający**: ${message.author.tag} \nID: ${message.author.id} \n\n🧭 **Serwer**: ${message.guild.name} \n**ID**: ${message.guild.id} \n\n✏️ **Treść**: ${args2} \n\n 📅 **Data**: ${today}`
      )
      .setFooter("Moduł reportowania bug'ów w Marketing");
    message.channel.send(bug);

    client.channels.cache.get("706063492867948565").send(bugADM);
    cooldownDelay = 15;
  }
  if (command === "pomoc") {
    author = message.author.tag;
    authorID = message.author.id;
    authorImgURL = message.author.avatarURL;

    const embed_simple = new Discord.MessageEmbed()
      .setTitle(
        "<a:Zweryfikowane:688397020624453701> Wysłano prywatną wiadomość"
      )
      .setFooter(
        "Komenda została wywołana przez: " +
          message.author.tag +
          " | ID: " +
          message.author.id,
        message.author.avatarURL()
      );

    const embed_commands = new Discord.MessageEmbed()
      .setTitle("<:MarketingLogo:688694803420282890> __**MARKETING**__")
      .setDescription(
        ":speech_balloon: Lista komend do bota **Marketing** \n\n``&r/&reklama [treść reklamy] - Ustawia reklame``  __**`(co 20 sek.)`**__" +
          "\n``&u/&ustaw #kanał - Ustawia kanał z reklamami``  __**`(co 15 sek.)`**__" +
          "\n``&jak - Poradnik dodania serwera do listy Marketingu``  __**`(co 5 sek.)`**__" +
          "\n``&zapros - Link ze swoim zaproszeniem do bota`` __**`(co 5 sek.)`**__\n" +
          "``&bug - Zgłasza błąd do administracji``  __**`(co 15 sek.)`**__\n" +
          "``&staty - Pokazuje statystyki serwera, kanał reklam, i czy reklama jest zaakceptowana``  __**`(co 5 sek.)`**__\n" +
          "``&wyswietl - Pokazuje reklamę serwera``  __**`(co 5 sek.)`**__"
      )
      .setFooter("© by Mondonno#6652 2020");

    message.channel.send(embed_simple);
    message.author.send(exampleEmbed);
    message.author.send(embed_commands);
    cooldownDelay = 5;
  }
}); // other commands...
// if(command === 'kick'){
//     if (!args.length) {
//               return message.channel.send(':eye: **Prawidłowe użycie komendy:** ``&kick @Uzytkownik``');
//             }
//         let member = message.mentions.members.first();
//         member.kick();
//         message.channel.send(`<:Ostrzezezenie:688690960116350996> Wyrzucono uytkownika ${member}`)
// }
// if(command === 'ban'){
//     if (!args.length) {
//               return message.channel.send(':eye: **Prawidłowe użycie komendy:** ``&ban @Uzytkownik``');
//             }
//         let member = message.mentions.members.first();
//         member.ban();
//         message.channel.send(`<:Ostrzezezenie:688690960116350996> Zbanowano uytkownika ${member}`)
// }

client.once("ready", async () => {
  console.log("Ready!");

  client.user.setActivity("&pomoc", {
    type: "PLAYING"
  });

  console.log(client.guilds.cache.size);
  console.log(client.users.cache.size);

  var i = 0;
  var channelDB;
  function myLoop() {
    setTimeout(function() {
      con.query(`SELECT * FROM Kanaly ORDER BY RAND() LIMIT 1;`, function(
        err,
        rows
      ) {
        if (err) throw err;
        if (!rows.length) {
          myLoop();
          return;
        }

        channelDB = rows[0].Kanal_ID;
      });
      con.query(`SELECT * FROM Reklamy where Zatwierdz = 0`, async function(
        err,
        rows
      ) {
        if (err) throw err;

        // if(!channel_2){
        //   myLoop()
        //   return
        //
        //  }else
        if (!rows.length) {
          myLoop();
          return;
        }
        //  if(i >= rows.length){
        //   i = 0;
        //   myLoop()
        // }

        //await Promise.all(client.channels.filter(c => c.id === rows[i].channel).map(c => c.send(`\`ID Reklamy:${rows[i].IDR}\n============\`\n\n`+rows[i].Reklama)))

        // console.log(rows[0].channel)

        if (!client.channels.cache.has(channelDB)) {
          myLoop();
          return;
        }
        if (!channelDB) {
          myLoop();
          return;
        }
        if (!rows[i].IDR) {
          myLoop();
          return;
        }
        if (!rows[i].Reklama) {
          myLoop();
          return;
        }
        var Wyslano_L = parseInt(rows[i].Wyslano_L);
        Wyslano_L++;

        await con.query(`Select * from Reklamy`, function(err, rows) {});
        await con.query(
          `UPDATE Reklamy SET Wyslano_L = '${Wyslano_L}' WHERE IDDS = '${rows[i].IDDS}'`,
          function(err, result, fields) {
            if (err) throw err;
            console.log(result);
          }
        );

        client.channels.cache
          .get(`${channelDB}`)
          .send(
            `**ID:** \`${rows[i].IDDS}\` **Nr.** \`${rows[i].IDR}\`\n\n\n` +
              rows[i].Reklama
          );
        i++;
        if (i < rows.length) {
          myLoop();
          Wyslano_L = 0;
        }
        if (i >= rows.length) {
          i = 0;
          Wyslano_L = 0;
          myLoop();
        }
      });
    }, 15000);
  }

  myLoop();
  //  start the loop
});
client.on("guildCreate", guild => {
  const leftEmbed = new Discord.MessageEmbed()
    .setAuthor(
      `Dodano bota na serwer ${guild.name}\nID: ${guild.id}\nID Właściciela: ${guild.ownerID}`,
      "https://cdn.discordapp.com/emojis/702810851995942922.gif?v=1"
    )
    .setThumbnail(guild.iconURL())
    .setColor("#00FF00")
    .setFooter(
      "Marketing™",
      "https://cdn.discordapp.com/avatars/688312861927276554/0d11ab4a20647e646ab45d6e3f0ac0c3.png?size=128"
    )
    .setTimestamp();
  client.channels.cache.get("688311675551612969").send(leftEmbed);
  //remove from guildArray
});
client.on("guildDelete", guild => {
  const leftEmbed = new Discord.MessageEmbed()
    .setAuthor(
      `Wyrzucono bota z serwer'a ${guild.name}\nID: ${guild.id}\nID Właściciela: ${guild.ownerID}`,
      "https://cdn.discordapp.com/emojis/702810788980588654.gif?v=1"
    )
    .setThumbnail(guild.iconURL())
    .setColor("#FF0000")
    .setFooter(
      "Marketing™",
      "https://cdn.discordapp.com/avatars/688312861927276554/0d11ab4a20647e646ab45d6e3f0ac0c3.png?size=128"
    )
    .setTimestamp();
  client.channels.cache.get("688311675551612969").send(leftEmbed);
  //remove from guildArray
});
client.on("guildMemberAdd", member => {
  if (member.guild.id == "688006607858434061") {
    member.send(
      "Witaj na ***Marketing™***  !\n\n> :pushpin: **Koniecznie przeczytaj <#688310079513952261> aby poznać nasze zasady**\n\n> :wave: **Przywitaj się na <#688006608630055007>**\n\n> :link: **No i zajrzyj na <#705371178956750879>**\n\n__`LINK DO SERWERA SUPPORT`__\nhttps://discord.gg/veR9UrF\n\n__`LINK DO DODANIA BOTA`__\nhttps://discordapp.com/oauth2/authorize?client_id=688312861927276554&permissions=8&scope=bot"
    );
  }
});
client.on("guildMemberRemove", member => {
  if (member.guild.id == "688006607858434061") {
    member.send(
      "**Szkoda że nas opuściłeś** :sob:  __Jeśli chciałbyś wrócić, wejdź do nas przez ten link__: https://discord.gg/veR9UrF"
    );
  }
});
// con.on('error', function(err) {
//   console.log("[mysql error]",err);
// });
// login to Discord with your app's token
client.login("Njg4MzEyODYxOTI3Mjc2NTU0.XtkL3w.6odiLztFJfMISg8Od5P8B2CL_Ro");

//Other

// for (var i = 0, len = rows.length; i < len; i++) {
//     client.channels.cache.get(channel_2.id).send(`\`ID Reklamy:\` ${rows[i].IDR} \n \`================
//     //       \``+"\n"+row[i].Reklama)
//     return
//     }

//     rows.forEach(row => {
//      if(!channel_2){
//          return
//        }else if(row.IDR < 1){
//    return
//     }else if(row.Reklama < 1){
// return
//  }

// client.channels.cache.get(channel_2.id).send(`\`ID Reklamy:\` ${row.IDR} \n \`================
// \``+"\n"+row.Reklama)
//  return
//  })
