

# Discord Bot

Make your own Discord Bot by following this tutorial step by step.


1. [Prerequisites](#1-prerequisites)


## 1] Prerequisites

First of all, let's get the needed tools:

#### > Java

In order to develop in Java you need to have installed the [Java Development Kit](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

We are going to use Java 8, and not a more recent version *cuz' all the other versions are lame*.

#### > IDE

I strongly recommend you using [IntelliJ IDEA](https://www.jetbrains.com/idea/download/). It's definitely the best IDE out there.



## 2] Creating a new IntelliJ IDEA project

1. Open `File > New > Project...`

![Step_1](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_1.png)

2. Select `Gradle` and then `Java`. 

> Make sure the **Project SDK** is set to `1.8`.

![Step_2](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_2.png)

3. Enter a **GroupId** and a **ArtifactId**.

> **GroupId** represents your company/developer id and **ArtifactId** represents your project's id.

![Step_3](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_3.png)

4. Enable `Use auto-import`.

![Step_4](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_4.png)

5. Enter your **Project name**.

![Step_5](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_5.png)

All you have to do now is to wait for Gradle to generate the project for you.



## 3] Configuring Gradle

We are now going to add our project's dependencies. Indeed, in order to communicate with Discord we would need to do some few things. Fortunately there's already some Java libraries which handle that for us. In our case, we're going to use [JDA](https://github.com/DV8FromTheWorld/JDA).

1. Open the **build.gradle** file.

![Step_6](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_6.png)

2. Modify the following parts:

```java
repositories {
    jcenter()
}

dependencies {
    compile ('net.dv8tion:JDA:3.8.3_462') {
        exclude module: 'opus-java'
    }

    testCompile group: 'junit', name: 'junit', version: '4.12'
}
```

You should now have:

![Step_7](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_7.png)

We finished adding our dependencies. The JDA library is now available for our project.



## 4] Coding the base of JDA Bot

1. Start by creating a new **Package** in `src/main/java`.

![Step_8](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_8.png)

2. Name it the following way `GroupId.ProjectId`. This is how you name your root package in Java.

![Step_9](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_9.png)

3. Create a new **Java Class** in the package we just created.

![Step_10](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_10.png)

4. Name it the following way `ProjectId` (starting with an uppercase). This is how you name your main class in Java.

![Step_11](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_11.png)

5. Open it and create the main method.

```java
public class Project_name {

    public static void main(String[] args) {
    }

}
```

Let's write the code to create a bot instance and connect it.

```java
public static void main(String[] args) {
    JDA jda;

    try {
        JDABuilder jdaBuilder = new JDABuilder(AccountType.BOT)
            .setToken("TOKEN")
            .setAudioEnabled(false)
            .setAutoReconnect(true);

        jda = jdaBuilder.build().awaitReady();
    } catch (InterruptedException e) {
        e.printStackTrace();
    } catch (LoginException e) {
        e.printStackTrace();
    }
}
```

>As you may have noticed we have edited two settings of the bot:
>
>**setAudioEnabled(false)**: the audio module has been disabled as we don't intend on using it.
>
>**setAutoReconnect(true)**: the auto-reconnect mode has been turned on. This is pretty useful as it will put the bot back online when ever it has lost it's Internet connection. (If ever it happens)
>
>There's plenty other settings you can change, you should take a bit of time to view what you can do.

Now that we have written the very base of the bot, let's create a Discord Bot account.



## 5] Creation of a Discord Bot account

1. Head to Discord's [Developer Portal](https://discordapp.com/developers/applications/).

![Step_12](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_12.png)

> If you don't already have a Discord account, you obviously need one for this. So go create one right now :)

2. Go in the **Applications** tab and press the `New Application` button.

![Step_13](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_13.png)

3. Enter a the desired Bot `Name`.

![Step_14](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_14.png)

4. *(Optional) Feel free to take some time to edit the Bot's profile giving it an avatar, a description, ...*

5. Go to the **Bot** tab and press a the *Build-A-Bot* `Add Bot` button.

![Step_15](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_15.png)

6. Press `Yes, do it!`.

![Step_16](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_16.png)

7. Once created, click on the `Click to Reveal Token` , and copy your bot's `Token`.

![Step_17](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_17.png)

8. In the main method, change the `TOKEN` string of the `.setToken("TOKEN")` with your Bot's token.



## 6] Adding event listeners

1. Create a **public static class** in your main class.

```java
public static class EventListener {
}
```

> Name it as you wish. For the sake of this demonstration, I will call it **EventListener**.

2. Make it **extend** the `ListenerAdapter` class from the JDA library.

```java
public static class EventListener extends ListenerAdapter {
}
```

> This will allow us to access Discord events that the bot received.

3. In order to receive events, we need to inform JDA of the existence of that class and create an instance of it as well. For this, add the following line into the **main method**.

```java
jdaBuilder.addEventListener(new EventListener());
```

You should have something like this:

```java
public static void main(String[] args) {
    JDA jda;

    try {
        JDABuilder jdaBuilder = new JDABuilder(AccountType.BOT)
            .setToken("TOKEN")
            .setAudioEnabled(false)
            .setAutoReconnect(true);

        jdaBuilder.addEventListener(new EventListener());

        jda = jdaBuilder.build().awaitReady();
    } catch (InterruptedException e) {
        e.printStackTrace();
    } catch (LoginException e) {
        e.printStackTrace();
    }
}
```

4. Let's now catch some few events. Edit the `EventListener` class with the following code.

```java
public static class EventListener extends ListenerAdapter {

    @Override
    public void onGuildJoin(GuildJoinEvent event) {
        event.getGuild().getDefaultChannel().sendMessage("I've just joined this server!").queue();
    }

    @Override
    public void onGuildLeave(GuildLeaveEvent event) {
        event.getGuild().getDefaultChannel().sendMessage("Bye everyone!").queue();
    }

    @Override
    public void onMessageReceived(MessageReceivedEvent event) {
        if (event.getMessage().getContentRaw().equals("ping")
            && event.getTextChannel().canTalk())
            event.getChannel().sendMessage("pong").queue();
    }
}
```

> The **onGuildJoin** method is triggered when ever the bot joins a new server and the **onGuildLeave** method when ever the bot leaves a server.
>
> The **onMessageReceived** method is triggered when ever the bot receives a message in any server he is in, but also when it receives direct messages.

Take some time to understand this code before continuing the tutorial.



## 7] Putting the Bot online

1. Click on `Add Configuration...` at the top right of IntelliJ IDEA.

![Step_18](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_18.png)

2. Press the `+` symbol in order to add a new configuration and select `Application`.

![Step_19](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_19.png)

3. Edit the following parts:

- `Name`: enter your **ArtifactId**.
- `Main class`: press the `...` and select your main class.
- `Use classpath of module`: select the **GroupeId.ArtifactId.main** module.
- `JRE`: just make sure it's using the Java 8 JRE (**1.8**).

![Step_20](https://raw.githubusercontent.com/Azzerial/KYT_CAT_Discord_Bot/master/.rsc/step_20.png)

4. Press `Apply` and then `OK`.

5. Press the play symbol or `Shift + F10`.

> To stop Application from running, just press the pause symbol or `Control + F2`.

We've just put the bot online. Let's see if the code we've written earlier works as expected.



## 8] Testing the code

1. Add your bot to a Discord server.

In order to do this, use the following URL:

```
https://discordapp.com/oauth2/authorize?client_id=BOT_ID&scope=ot&permissions=8
```

> Replace `BOT_ID` by your Bot's Id.

Your Bot should have said `I've just joined this server!` in the `#general` channel.

2. Try saying `ping`.

The Bot should have replied with `pong`.

3. Kick the Bot from the server.

It should have said `Bye everyone!` before leaving the server.



## 9] Creating a command management system

1. Create a new class and name it `Command`.
2. Make it extend the `ListenerAdapter` class.

> As the Command class will manage the commands the bot has received, it need to have access to the JDA events.

3. In order to make it easy to create new commands, we'll make the `Command` class **Abstract** and let the actual commands classes extends the `Command` class.

   For this reason, make the `Command` class abstract by adding the `abstract` keyword before the `class` one.

> You should have something like this:
>
> ```java
> public abstract class Command extends ListenerAdapter {
> }
> ```

4. As the class is now abstract, we can add some abstract methods in order to get some information about the extending class.

> An abstract method is a method that *must be overwritten* by the extending class. As for an interface, the method doesn't have a body and needs to have one in the class implementing the method.

Let's add the following methods:

```java
public abstract String getName();
public abstract String getDescription();
public abstract List<String> getAliases();
public abstract void onCommand(MessageReceivedEvent event, String[] args);
```

Thanks to these, we'll be able to get some information on the extending class directly in the abstract one.

5. Override the `onMessageReceived` method and add the following code.

```java
public static final String PREFIX = "./";

@Override
public void onMessageReceived(MessageReceivedEvent event) {
    if (event.getAuthor().isBot() || !event.isFromType(ChannelType.TEXT))
        return;
    String[] args = event.getMessage().getContentRaw().split(" ");
    if (isMessageFromCommand(args[0]))
        onCommand(event, args);
}

private boolean isMessageFromCommand(String keyword) {
    if (keyword.startsWith(PREFIX))
        keyword = keyword.substring(PREFIX.length());
    else
        return (false);
    return (getAliases().contains(keyword));
}
```

First of all, we check if the command has been sent by a user and not an other Bot, as well as if it have been sent in a **TextChannel** which represents a text channel within a guild. If ever it's not the case, then we won't take this message into account.

```java
if (event.getAuthor().isBot() || !event.isFromType(ChannelType.TEXT))
    return;
```

Then we create a **String** array with contains all of the messages words.

```java
String[] args = event.getMessage().getContentRaw().split(" ");
```

At last, we check thanks to the `isMessageFromCommand` method is the message sent was destined to the class extending `Command`, before calling the `onCommand` method.

```java
if (isMessageFromCommand(args[0]))
    onCommand(event, args);
```

The `isMessageFromCommand` method basically checks whether or not the keyword of the command starts with the Bot's command prefix. If it's the case then it will check whether or not the keyword (without the command prefix) corresponds to one of the aliases of the extending class.



Our class should look like this

```java
public abstract class Command extends ListenerAdapter {

    public static final String PREFIX = "./";

    public abstract String getName();
    public abstract String getDescription();
    public abstract List<String> getAliases();
    public abstract void onCommand(MessageReceivedEvent event, String[] args);

    @Override
    public void onMessageReceived(MessageReceivedEvent event) {
        if (event.getAuthor().isBot() || !event.isFromType(ChannelType.TEXT))
            return;
        String[] args = event.getMessage().getContentRaw().split(" ");
        if (isMessageFromCommand(args[0]))
            onCommand(event, args);
    }

    private boolean isMessageFromCommand(String keyword) {
        if (keyword.startsWith(PREFIX))
            keyword = keyword.substring(PREFIX.length());
        else
            return (false);
        return (getAliases().contains(keyword));
    }
}
```



## 10] Adding Commands

1. *(Optional) I suggest creating a package called `commands` in which we'll store all the created command classes.*
2. Add a new class named the following way `NameCommand`.

**Name** represents the command's name, and we append it with  `Command`  in order to clarify code and view quickly that this class extends the `Command` one.

> Example:
>
> `PingCommand`

3. Make it extend the `Command` class.

> Example:
>
> ```java
> public class PingCommand extends Command
> ```

4. Implement the abstract methods from `Command` and enter the needed information.

> Press `Control + I` in order to generate them with IntelliJ IDEA.

Enter the class's name in `getName` method.

> Example:
>
> ```java
> @Override
> public String getName() {
>        return ("Ping");
> }
> ```

Enter a short description of the command's action, or usage, in the `getDescription` method.

> Example:
>
> ```java
> @Override
> public String getDescription() {
>        return ("Replies with \"pong\"");
> }
> ```

List the command's keywords in the `getAliases` method.

> Example:
>
> ```java
> @Override
> public List<String> getAliases() {
>        return (Arrays.asList(
>            "p",
>            "ping"
>        ));
> }
> ```

Code the command's behavior in the `onCommand` method.

> Example:
>
> ```java
> @Override
> public void onCommand(MessageReceivedEvent event, String[] args) {
>        if (event.getTextChannel().canTalk())
>            event.getTextChannel().sendMessage("pong").queue();
> }
> ```



## 11] Command examples

#### > PingCommand

```java
public class PingCommand extends Command {

    @Override
    public String getName() {
        return ("Ping");
    }

    @Override
    public String getDescription() {
        return ("Replies with \"pong\"");
    }

    @Override
    public List<String> getAliases() {
        return (Arrays.asList(
            "p",
            "ping"
        ));
    }

    @Override
    public void onCommand(MessageReceivedEvent event, String[] args) {
        if (event.getTextChannel().canTalk())
            event.getTextChannel().sendMessage("pong").queue();
    }
}
```

#### > PurgeCommand

```java
public class PurgeCommand extends Command {

    @Override
    public String getName() {
        return ("Purge");
    }

    @Override
    public String getDescription() {
        return ("Purges <amount> messages in the channel.");
    }

    @Override
    public List<String> getAliases() {
        return (Arrays.asList(
            "clean",
            "clear",
            "delete",
            "purge",
            "remove"
        ));
    }

    @Override
    public void onCommand(MessageReceivedEvent event, String[] args) {
        if (!event.getTextChannel().canTalk()
            || !event.getGuild().getMember(event.getJDA().getSelfUser()).hasPermission(event.getTextChannel(), Permission.MESSAGE_HISTORY, Permission.MESSAGE_MANAGE))
            return;
        if (args.length != 2) {
            event.getTextChannel().sendMessage(
                "Usage: " + PREFIX + "purge <amount>\n" +
                "\tamount\tnumber of messages to purge"
            ).queue();
            return;
        }
        int amount = Integer.parseInt(args[1]);
        event.getTextChannel().getHistoryBefore(event.getMessage(), amount).queue(messageHistory -> {
            messageHistory.getRetrievedHistory().forEach(message -> {
                message.delete().queue();
            });
        });
        event.getMessage().delete().queue();
        event.getTextChannel().sendMessage("Successfully purged " + amount + " messages.").queue();
    }
}
```

#### > Respects

```java
public class RespectsCommand extends ListenerAdapter {

    private static final HashMap<Long, AtomicLong> counter;

    public String getName() {
        return ("Respects");
    }

    public String getDescription() {
        return ("Press F to pay respects.");
    }

    @Override
    public void onMessageReceived(MessageReceivedEvent event) {
        if (event.getAuthor().isBot() || !event.isFromType(ChannelType.TEXT))
            return;
        if (!event.getTextChannel().canTalk()
            || !event.getMessage().getContentRaw().equalsIgnoreCase("f"))
            return;
        long id = event.getAuthor().getIdLong();
        if (!counter.containsKey(id))
            counter.put(id, new AtomicLong(1));
        event.getTextChannel().sendMessage(event.getAuthor().getName() + " has paid their respects for the " + counter.get(id).getAndIncrement() + " time.").queue();
    }

    static {
        counter = new HashMap<Long, AtomicLong>();
    }
}
```

