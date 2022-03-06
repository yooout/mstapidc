# mstapidc
API responsável por retornar informações de qualquer usuário do Discord.


```js
const request = require('request-promise')

async function getUser(id) {

    let usuario;


    var result = await request.get({
        url: `https://discord.com/api/v9/users/${id}/profile`,
        json: true,
        headers: {
            'Authorization': "token"
        }
    }, async (error, body, response) => {

        if (body.statusCode == 200) {

            let badgesUser = [];

            var array = ["1", "2", "4", "8", "64", "128", "256", "512", "16384", "131072"]


            for (i in array) {
                if ((response.user.flags & array[i]) == array[i]) {

                    var checked = {
                        1: "TEAM_USER",
                        2: "PARTNERED_SERVER_OWNER",
                        4: "HYPESQUAD_EVENTS",
                        8: "BUGHUNTER_LEVEL_1",
                        64: "HOUSE_BRAVERY",
                        128: "HOUSE_BRILLIANCE",
                        256: "HOUSE_BALANCE",
                        512: "EARLY_SUPPORTER",
                        16384: "BUGHUNTER_LEVEL_2",
                        131072: "EARLY_VERIFIED_BOT_DEVELOPER"
                    }
                    badgesUser.push(checked[array[i]])

                }
                else {

                }
            }
            response.user.flags = badgesUser

        }
    })
    usuario = result.user
    usuario.tag = usuario.username + "#" + usuario.discriminator

    usuario.displayAvatarURL = function () {
        if (usuario.avatar.indexOf('a_') > -1) {
            return "https://cdn.discordapp.com/avatars/" + usuario.id + "/" + usuario.avatar + ".gif"
        } else {
            return "https://cdn.discordapp.com/avatars/" + usuario.id + "/" + usuario.avatar + ".png"
        }
    }

    return usuario

}
```
