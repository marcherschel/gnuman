# Gnuman - Satirical Pacman Clone with Map Editor using JavaFX

## Update Feb 2020: Project has been updated to Java 11 due to being requested.

As our 3rd semester project we had to make a game, I choose to create a Pacman clone. I have no prior experience with game development and never created a GUI application before so the implementation might be a bit messy.

The large.gnuman is a 125x125 cell map, if you want to test this map I recommend turning the dynamic camera on, otherwise the map is too large to display properly.

# Description

You are maneuvering Richard Stallman through the labyrinth that is the development of free software, dodging all these mega corporations trying to prevent you from releasing an alternative to their products.

This game is a satire and should be taken with a grain of salt in case you're a die hard capitalist or the type that gets all offended when a cooperation is not always represented as the marketing team would want it to.

While I do agree with lots of RMSs standpoints it should be noted that I'm neither making a political statement with this game nor am I here to fully represent his view of the world nor am I insulting him.

I tried to emulate Pacman as good as I could. I hope you can enjoy this game =)

# Requirements

~~For this version you'll need the Java 8 runtime. The OpenJDK 8 might work but I'm not sure since JavaFX is required. There is also a Java 11 version that seems to work well under Win and most Linux systems. The Java 11 version is much larger as it is packed with all the native libraries to guarantee cross platform compatibility. It works with OpenJDK 11 and JavaSE 11.~~

1.0.9 requires Java 11. Older versions need Java 8 Oracle JRE/JDK.

# [Get the .jar here!](https://github.com/mynttt/gnuman/releases/latest)

# Thanks to

- [Gamasutra for releasing an in-depth guide on the internals of Pacman and all the rules the game follows.](https://web.archive.org/web/20160727210445/http://www.gamasutra.com/view/feature/3938/the_pacman_dossier.php)

- Whoever wrote the Stallman Facts

---

# Blog Article from my old Website

Gnuman is a satirical Pacman clone written in Java utilizing JavaFX for the graphical user interface. It features a map editor, allowing players to design their own game worlds.

In Gnuman, you play as Richard Stallman, maneuvering through the labyrinth of free software development. Your goal is to collect all the Git artifacts needed to release a new version of your software while avoiding the corporations pursuing you. They have no interest in seeing you release a product that might disrupt their carefully planned business model by providing a better and libre alternative.

![Gnuman main menu screen](https://github.com/user-attachments/assets/3d4add7a-932b-4327-aafe-545676002126)

The game attempts to emulate the original Pac-Man as faithfully as possible by implementing the original timings, ghost chase and scatter behavior, point system, and gameplay modifiers as described in the [in-depth Pac-Man dossier by Gamasutra](https://web.archive.org/web/20160727210445/http://www.gamasutra.com/view/feature/3938/the_pacman_dossier.php)

Not every quirk of the original game is replicated. For example, the level counter is stored as a 32-bit signed integer rather than overflowing at **256** as it did in the arcade version. Even if you somehow reached level **2,147,483,647**, the overflow would not recreate the infamous split-screen bug.

| Gameplay | Victory |
|----------|----------|
| ![Gameplay](https://raw.githubusercontent.com/mynttt/gnuman/master/img/game.PNG "Game Screen") | ![Finish screen](https://github.com/user-attachments/assets/720614c7-526b-4f87-b553-e0a515374b4a) |

The menu system is built with JavaFX's FXML technology, while the gameplay itself is rendered on a canvas. To keep performance smooth on very large maps, the game uses chunking for collision detection. An example is the included [125×125 map](https://github.com/mynttt/gnuman/blob/master/large.gnuman).

https://github.com/user-attachments/assets/94702f8b-d696-4d5d-b75a-8fe980fcdf62

Internally, the ghosts are implemented as state machines with the movement states:

- `WAITING`
- `SCATTER`
- `FRIGHTENED`
- `CHASE`
- `DEAD`
- `COOLDOWN`
- `LEAVE_BASE`

State transitions occur either when the player triggers an event (such as collecting the GPL License power-up) or when timers alternate the ghosts between `SCATTER` and `CHASE`.

A second state machine controls collision behavior with the following states:

- `NORMAL`
- `DEAD`
- `FRIGHTENED`

During `SCATTER`, ghosts target unreachable points outside the map, causing them to endlessly patrol their respective corners. The `CHASE` behavior closely follows the original Pac-Man:

- **Blinky** directly pursues the player.
- **Pinky** attempts to ambush.
- **Inky** alternates between chasing, ambushing, and unpredictable movement.
- **Clyde** retreats to his corner whenever he gets too close.

Blinky also [increases his speed](https://github.com/mynttt/gnuman/blob/master/src/main/java/de/hshannover/inform/gnuman/app/rules/ElroyRules.java) twice during each level depending on the number of remaining dots, forcing players to finish quickly before he becomes faster than the player.

Difficulty scales with each level by:

- Adjusting the [player and ghost speed modifiers](https://github.com/mynttt/gnuman/blob/master/src/main/java/de/hshannover/inform/gnuman/app/rules/EntitySpeedRules.java)
- [Reducing frightened mode duration](https://github.com/mynttt/gnuman/blob/master/src/main/java/de/hshannover/inform/gnuman/app/rules/GhostFrighteningRules.java)
- [Changing SCATTER/CHASE timing patterns](https://github.com/mynttt/gnuman/blob/master/src/main/java/de/hshannover/inform/gnuman/app/rules/GhostStateRules.java)

https://github.com/user-attachments/assets/a23bef07-6672-42d9-beff-9243de05cfd3

The video above displays the game's internal workings using a debug renderer. The moving green square represents the player, while the ghosts are color-coded:

- **Red:** Blinky
- **Pink:** Pinky
- **Turquoise:** Inky
- **Yellow:** Clyde

## Map Editor

Gnuman also includes a fully featured map editor that can be launched from the **Extra** menu. Players can create custom levels up to a maximum size of **125×125** tiles, which is intentionally limited because larger maps tend to reduce gameplay quality.

The image below shows the editor with a 125×125 map loaded.

![Gnuman map editor](https://github.com/user-attachments/assets/d6e3fe67-1829-468a-ac09-8e81f5f3961e)

The final video demonstrates the editor's features, including creating a custom level, loading the large 125×125 map, and showcasing both the static and dynamic camera renderers.

https://github.com/user-attachments/assets/25546900-1ce7-4591-ba1b-40a0678e53e1
