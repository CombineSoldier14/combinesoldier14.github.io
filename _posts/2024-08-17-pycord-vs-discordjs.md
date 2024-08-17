---
layout: post
title: Pycord vs discord.js
---

![Pycord vs Discord.js](https://postimg.cc/HVgJDYj6)

Well, this is certainly one way to make a grand return.

Anyway, I'm sure most of you know I really love discord bots. I've made bots on three seperate platforms (Bot Designer for Discord, Pycord, and discord.js) and I have gotten good experience with all of them. Today, I'm here to compare them and how they work, giving a good idea to everyone of who these are meant for, how they work, etc.

## Important Things!
* This is NOT and should not be considered a guide for any of these libraries. I will be explaining how they work and how they are usually structured, but compared to the [Pycord guide](https://guide.pycord.dev/) and [discord.js](https://discordjs.guide/) guide, these aren't as good.
* I will be using Pycord over discord.py as it's more actively maintained and efficent.
* I will be using slash commands over prefixed commands, as they are much easier and efficent to work with.
* This is meant for people that have a good grasp on both Python and JavaScript.
* I will be using global commands over guild commands for command creation here.
* I won't be covering any voice/music libraries
* I won't be doing a *huge* dive into these libraries, just the basics to see how they function.
* Even though Bot Designer for Discord was mentioned earlier, I will not be including it as I do not consider it good anymore for a variety of reasons. You can read up on their [docs](https://wiki.botdesignerdiscord.com/) if you want.

## Part I: History
discord.js was created in August 2015 as a small project by [Amish Shah](https://gist.github.com/amishshah) as a simple discord API wrapper. It heavily grew in popularity, and became hugely maintained by its community.

Pycord was created in 2020 as a actively maintained fork of Discord.py that at the time, was thought to be dead. Pycord outgrew its predecessor over the years and is now a very popular and easy way to create bots.

## Part II: Installation
Both of the libraries can be installed easily. For Python, you can use their `pip` package manager to get Pycord:
`pip install pycord`

For discord.js, you will need to have [node.js](https://nodejs.org/en/download/package-manager) along with `npm` installed:
`npm install discord.js`

## Part III: Client Intitalization
I'll be starting this assuming you've already created your bot at the [Discord Developer Portal](https://discord.com/developers). Considering Pycord is written with Python and discord.js is written in JavaScript, the syntax is obviously going to be different.

In Pycord, here is the basic way to initalize a client in your `main.py` file:
```python
import discord
import os # default module

bot = discord.Bot()

@bot.event
async def on_ready():
    print(f"{bot.user} is ready and online!")

bot.run('TOKEN')
```
The `@bot.event` is optional. I put it there as it's good practice, but here is the basic way to create a bot. It's very simple. Create a new `discord.Bot()` instance, and run it with your token at the bottom. It's extremely simple, yet effective.

In discord.js, it's a bit more complicated in your `index.js` file:
```javascript
// Require the necessary discord.js classes
const { Client, Events, GatewayIntentBits } = require('discord.js');

// Create a new client instance
const client = new Client({ intents: [GatewayIntentBits.Guilds] });

// Log in to Discord with your client's token
client.login(token);
```
We can import the `client`, `events`, and `GatewayIntentBits` properties from discord.js. The `client` instance is a bit different here. In pycord, it's a simple `discord.Bot()`. Here we have to intialize a new `client` instance and define the intents used.
Discord intents are needed for their gateway API (what we use to make bots with), which allow us to get access to content on the API. An interesting thing to note is we define our intents in a JSON request, as we are actually making a low-level API request, something discord.js is known for.

## Part IV: Adding Commands
So we've seen how to create a client instance to run. Great. But what about what discord bots are known for, *commands*?
In Pycord, adding commands is simple:
```python
import discord
import os # default module

bot = discord.Bot()

@bot.event
async def on_ready():
    print(f"{bot.user} is ready and online!")

@bot.slash_command(name="hello", description="Say hello to the bot")
async def hello(interaction: discord.Interaction):
    await interaction.response.send_message("Hey!")

bot.run('TOKEN')
```
We can use a decorator to start a slash command where we specify the name and description. Under that, we make an `async` function with `interaction` as an argument. Then, the function will `response` with `send_message` on `interaction` with your text. Easy and straightforward.

Discord.js is more complicated. You must create a seperate `.js` file that imports discord.js and exports a `SlashCommandBuilder()` object:
```javascript
const { SlashCommandBuilder } = require('discord.js');

module.exports = {
	data: new SlashCommandBuilder()
		.setName('ping')
		.setDescription('Replies with Pong!'),
	async execute(interaction) {
		await interaction.reply('Pong!');
	},
};
```
Here, we can set the properties of `SlashCommandBuilder()` and create an `async` function called `execute` with `interaction` as an argument, similar to Pycord, where we use the `reply` method to reply back with text. Alternatively, you can make a lower-level JSON with your text content:
```javascript
await interaction.reply({ content: 'Pong!' })
```
With all this, we can see in greater detail *how* discord is operating here, by exporting slash commands to the main file. But we need to handle those slash commands so they get recognized:
```javascript
const fs = require('node:fs');
const path = require('node:path');
const { Client, Collection, GatewayIntentBits } = require('discord.js');

const client = new Client({ intents: [GatewayIntentBits.Guilds] });

client.commands = new Collection();
const foldersPath = path.join(__dirname, 'commands');
const commandFolders = fs.readdirSync(foldersPath);

for (const folder of commandFolders) {
	const commandsPath = path.join(foldersPath, folder);
	const commandFiles = fs.readdirSync(commandsPath).filter(file => file.endsWith('.js'));
	for (const file of commandFiles) {
		const filePath = path.join(commandsPath, file);
		const command = require(filePath);
		if ('data' in command && 'execute' in command) {
			client.commands.set(command.data.name, command);
		} else {
			console.log(`[WARNING] The command at ${filePath} is missing a required "data" or "execute" property.`);
		}
	}
}

const eventsPath = path.join(__dirname, 'events');
const eventFiles = fs.readdirSync(eventsPath).filter(file => file.endsWith('.js'));

for (const file of eventFiles) {
	const filePath = path.join(eventsPath, file);
	const event = require(filePath);
	if (event.once) {
		client.once(event.name, (...args) => event.execute(...args));
	} else {
		client.on(event.name, (...args) => event.execute(...args));
	}
};

client.login('TOKEN');
```
Of course, this can be done in many other ways. We create a new `collection()` instance, which keeps a *collection* of slash commands. We then set up ways to get to the command folder (in this example we kept the slash command file earlier in `./commands/utility/ping.js`.)
We then use a nested loop to go through and get the right file where we finally set it in `client.commands`. We also see another thing called `eventFiles`, which we will discuss now. That is used to get files from a folder called `events` and execute their events. In this folder we have two files:
`ready.js`: used to execute when the bot is ready (optional, but good practice)
```javascript
const { Events } = require('discord.js');

module.exports = {
	name: Events.ClientReady,
	once: true,
	async execute(client) {
		console.log(`Ready! Logged in as ${client.user.tag}`);
	},
};
```
And then `interactionCreate.js` for interaction and error handling:
```javascript
const { Events } = require('discord.js');

module.exports = {
	  client.on(Events.InteractionCreate, async interaction => {
	  if (!interaction.isChatInputCommand()) return;

	  const command = interaction.client.commands.get(interaction.commandName);

	  if (!command) {
		  console.error(`No command matching ${interaction.commandName} was found.`);
		  return;
	  }

	  try {
		  await command.execute(interaction);
	  } catch (error) {
		  console.error(error);
		  if (interaction.replied || interaction.deferred) {
			  await interaction.followUp({ content: 'There was an error while executing this command!', ephemeral: true });
		  } else {
			  await interaction.reply({ content: 'There was an error while executing this command!', ephemeral: true });
		  }
	  }
  });
};
```
Here, they are exporting events where we can see that they handle errors. It's important to note that when running a slash command, discord looks for a `reply` to the slash command, rather than normal `send` or `followUp` messages. Slash commands can only be replied to once and even if it only returns a `send` message, the command will still say it failed.
Again, these are all optional. They are simply good practice. You don't even need to have these events in seperate files, I've seen some people have all their handlers and events in `index.js`, which isn't as efficent but works too. I'm simply showing how these bots can be spread across many different files.
Finally, we must deploy these commands to discord. While this *can* be done in `index.js` in a manner similar to Pycord, it's better to have its own script so you can update Discord without having to run the bot and choose when the commands go public.
```javascript
const { REST, Routes } = require('discord.js');
const fs = require('node:fs');
const path = require('node:path');

const commands = [];
// Grab all the command folders from the commands directory you created earlier
const foldersPath = path.join(__dirname, 'commands');
const commandFolders = fs.readdirSync(foldersPath);

for (const folder of commandFolders) {
	// Grab all the command files from the commands directory you created earlier
	const commandsPath = path.join(foldersPath, folder);
	const commandFiles = fs.readdirSync(commandsPath).filter(file => file.endsWith('.js'));
	// Grab the SlashCommandBuilder#toJSON() output of each command's data for deployment
	for (const file of commandFiles) {
		const filePath = path.join(commandsPath, file);
		const command = require(filePath);
		if ('data' in command && 'execute' in command) {
			commands.push(command.data.toJSON());
		} else {
			console.log(`[WARNING] The command at ${filePath} is missing a required "data" or "execute" property.`);
		}
	}
}

// Construct and prepare an instance of the REST module
const rest = new REST().setToken('TOKEN');

// and deploy your commands!
(async () => {
	try {
		console.log(`Started refreshing ${commands.length} application (/) commands.`);

		// The put method is used to fully refresh all commands in the guild with the current set
		const data = await rest.put(
			Routes.applicationCommands('CLIENT_ID'),
			{ body: commands },
		);

		console.log(`Successfully reloaded ${data.length} application (/) commands.`);
	} catch (error) {
		// And of course, make sure you catch and log any errors!
		console.error(error);
	}
})();
```
So finally we can finish our system for commands. It's obviously a lot more complicated than Pycord, but don't take that as it being bad! It can give a lot more freedom with how you interact with the API and of course, spread out events, handling, and commands.
However, if you think that Pycord can't do that, you are wrong. While Pycord can also export events in a similar fashion, the library also has a feature known as `cogs`, or `extensions`. They are seperate `.py` files that can be loaded onto `main.py`:
```python
import discord
from discord.ext import commands

class Greetings(commands.Cog): # create a class for our cog that inherits from commands.Cog
    # this class is used to create a cog, which is a module that can be added to the bot

    def __init__(self, bot): # this is a special method that is called when the cog is loaded
        self.bot = bot

    @discord.slash_command() # we can also add application commands
    async def goodbye(self, ctx):
        await ctx.respond('Goodbye!')

    @commands.Cog.listener() # we can add event listeners to our cog
    async def on_member_join(self, member): # this is called when a member joins the server
    # you must enable the proper intents
    # to access this event.
    # See the Popular-Topics/Intents page for more info
        await member.send('Welcome to the server!')

def setup(bot): # this is called by Pycord to setup the cog
    bot.add_cog(Greetings(bot)) # add the cog to the bot
```
In here, we can create a class called `Greetings` in a seperate file that we can import Pycord into and create commands, as well as event listeners, which listen to events and respond when they happen. This is all very useful for creating command groups and optimizing your bot. While I personally prefer discord.js's method of having each command be their own file and object, this is also a great way to spread out a bot.
In `main.py`, add something like this to load the extension:
```python
bot.load_extension('cogs.greetings')
```

## Part V: Options and Embeds
While we have so far covered the basics of slash commands, more advanced command creation involves adding slash command options, which provide data for the command, and embeds, which are stylized HTML boxes that can be made in discord.
In discord.js, you have a `EmbedBuilder()` object similar to `SlashCommandBuilder()`:
```javascript
// inside a command, event listener, etc.
const exampleEmbed = new EmbedBuilder()
	.setColor(0x0099FF)
	.setTitle('Some title')
	.setURL('https://discord.js.org/')
	.setAuthor({ name: 'Some name', iconURL: 'https://i.imgur.com/AfFp7pu.png', url: 'https://discord.js.org' })
	.setDescription('Some description here')
	.setThumbnail('https://i.imgur.com/AfFp7pu.png')
	.addFields(
		{ name: 'Regular field title', value: 'Some value here' },
		{ name: '\u200B', value: '\u200B' },
		{ name: 'Inline field title', value: 'Some value here', inline: true },
		{ name: 'Inline field title', value: 'Some value here', inline: true },
	)
	.addFields({ name: 'Inline field title', value: 'Some value here', inline: true })
	.setImage('https://i.imgur.com/AfFp7pu.png')
	.setTimestamp()
	.setFooter({ text: 'Some footer text here', iconURL: 'https://i.imgur.com/AfFp7pu.png' });

channel.send({ embeds: [exampleEmbed] });
```
Here, we can customize the many properties of a embed and then send a JSON response with the embed included. Pycord, on the other hand, has a slightly more high-level approach:
```python
embed = discord.Embed(
        title="My Amazing Embed",
        description="Embeds are super easy, barely an inconvenience.",
        color=discord.Colour.blurple(), # Pycord provides a class with default colors you can choose from
    )
    embed.add_field(name="A Normal Field", value="A really nice field with some information. **The description as well as the fields support markdown!**")

    embed.add_field(name="Inline Field 1", value="Inline Field 1", inline=True)
    embed.add_field(name="Inline Field 2", value="Inline Field 2", inline=True)
    embed.add_field(name="Inline Field 3", value="Inline Field 3", inline=True)
 
    embed.set_footer(text="Footer! No markdown here.") # footers can have icons too
    embed.set_author(name="Pycord Team", icon_url="https://example.com/link-to-my-image.png")
    embed.set_thumbnail(url="https://example.com/link-to-my-thumbnail.png")
    embed.set_image(url="https://example.com/link-to-my-banner.png")
 
    await interaction.response.send_message("Hello! Here's a cool embed.", embed=embed)
```
Here, we call the method `discord.Embed` with some base arguments and then add to its properties with fields, footers, and more. Again, they are both similar approaches.

## Part VI: Conclusions
We have now seen the basics of both Pycord and discord.js. So let's compare:

**discord.js:** An advanced JavaScript library for the discord API with a lot of freedom. You have to write more of the code yourself and interact with the API directly. This is harder, but can also be very benefical as there are now a lot of ways you can handle commands/events and you can of course split them into many different files. Also is considered to be more stable and feature-rich than Pycord.
**Pycord:** A Python library for the discord API that is simple and great for beginners. It does most of the heavy lifting and can also handle splitting commands/extensions across itself. It is newer, and may be more prone to bugs, but makes good use of the Python syntax in an intuitive way.

Overall, I would say discord.js is better for the experienced developer who wants more control over their bot interactions, while Pycord is better for beginners or people who want simple bot projects and don't care too much about extra features/control.

My personal pick? It's hard to say, as I both like them, but I think it's **discord.js**. It grants me more control over handling interactions while giving stability and features.

Anyway, thanks for reading! I know I haven't been posting much lately, so I'd figure I'd satisfy you all with this gigantic essay.
