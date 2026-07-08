# Editing Files - Task Completion

## Task: Exercise - Run the Vim Tutorial

In this exercise, I ran the Vimtutor application and followed all the directions in the file for tasks 1-4. Vimtutor is an application that teaches the basics of how to use Vim, which is one of the text editors for Linux.


### Step 1: Starting the Vimtutor Session

From my current location in the terminal, I entered `vimtutor` and pressed Enter. This started the vimtutor session.

```bash
vimtutor
```

**Screenshot of vimtutor startup:**

<img width="771" height="247" alt="Screenshot 2026-06-18 182420" src="https://github.com/user-attachments/assets/7b91d9b7-3456-4fe0-ad76-0e295b280d44" />


I successfully launched the Vim tutorial and saw the welcome screen that explained Vim is a very powerful editor with many commands. The tutorial indicated it was designed to describe enough of the commands to easily use Vim as an all-purpose editor, with an approximate completion time of 30 minutes.

### Step 2: Completing Vimtutor Lessons

I completed lessons 1-3 in vimtutor. The terminal window displayed output from the command vimtutor, showing tutorial lesson 1.1 and moving the cursor from vimtutor with the appropriate navigation commands.

### Step 3: Exiting Vimtutor

I entered `:q!` and pressed Enter to exit vimtutor and return to the main terminal.

---

After the Vimtutor I first needed to see that vim was was installed before moving on to editing and creating a file.

<img width="785" height="205" alt="Screenshot 2026-06-18 183138" src="https://github.com/user-attachments/assets/9dd470a9-a123-46cb-a0fa-ebaee793c6db" />

## Task 3: Exercise - Edit a File in Vim

In this exercise, I used the Vim command-line editor program to create and edit a file using the following steps.

### Step 1: Creating and Opening the File

From my current location in the terminal, I entered `vim helloworld` and pressed Enter. I was using Vim to create a file called `helloworld`.

```bash
vim helloworld
```
The terminal window displayed output from the command `vim helloworld`, showing Vim was opened and ready for editing.

### Step 2: Entering Text in Insert Mode

Now that I was in the file I created called `helloworld`, I used Vim to insert a few lines of text. I entered `i` to use insert mode and entered the following text:

```
Hello World!
This is my first file in Linux and I am editing it in Vim!
```

**Screenshot of file content in Vim:**

<img width="630" height="75" alt="Screenshot 2026-06-18 183106" src="https://github.com/user-attachments/assets/3b07827c-d469-4ece-92fb-17e30fa51a10" />


I successfully added the text to the helloworld file. The cursor was positioned at the beginning of the new lines, and the text appeared exactly as I typed it.

### Step 3: Saving and Exiting

Once complete, I pressed ESC to exit insert mode. Then I used the following command to save and quit:

```bash
:wq
```

This saved my changes to the file and exited Vim, returning me back to the main terminal.

### Step 4: Reopening and Editing the File

I was back in the main terminal. I used Vim to go back to the `helloworld` file and analyze what happened. I entered `vim helloworld` and pressed Enter to open the file again.

```bash
vim helloworld
```



### Step 5: Adding Additional Content

I was back in the file that I previously created called `helloworld`. I used Vim to add the following line to the editor:

```
I learned how to create a file, edit and save them too!
```

The terminal displayed the content I had previously entered in the helloworld file. The second line now displayed the new text I added to the file.

### Step 6: Final Save and Exit

Once complete, I pressed ESC to exit insert mode and used the following command to save changes without quitting:

```bash
:w
```

Then I exited with:

```bash
:q!
```

I was back in the main terminal. I used Vim to go back to the `helloworld` file and analyze what happened. What was the difference? The file now contained both my original text and the new line I added, demonstrating that Vim successfully saved my changes between sessions.

---

## Additional Challenge

I tried additional useful commands to enhance my Vim skills.

### Delete Entire Line Command

I used the following command to delete an entire line:

```bash
dd
```

### Undo Last Command

I used the following command to undo the last command:

```bash
u
```

### Save Without Quitting

I used the following command to save changes without quitting:

```bash
:w
```

---

## Task 4: Exercise - Edit a File in Nano

In this exercise, I used an alternative command-line editor program called nano. I used nano to create and edit a text file.

### Step 1: Creating the File with Nano

Similar to Vim, in the main terminal, I entered `nano cloudworld` and pressed Enter. I was using nano to create a file called `cloudworld`.

```bash
nano cloudworld
```

The terminal window displayed the command nano followed by the word cloudworld. Unlike Vim, I did not have to enter insert mode. Instead, I could start typing immediately.

### Step 2: Entering Text

Now that I was in the file I created called `cloudworld`, unlike Vim, I did not have to enter insert mode. Instead, I could simply start typing. I entered the following text:

```
We are using nano this time! We can simply start typing! No insert mode needed.
```

<img width="813" height="521" alt="Screenshot 2026-06-18 184115" src="https://github.com/user-attachments/assets/92ca41db-5c99-48c2-9d55-c69963fbda9a" />

I successfully added the text to the cloudworld file while in the nano editor. The nano interface showed the file content with the text I typed displayed on the screen, and at the bottom of the terminal, the keyboard shortcuts for common nano commands were visible.

### Step 3: Saving the File

To save my changes to the file, I pressed `CTRL+O`. I pressed Enter to confirm the file name once I saved it.

### Step 4: Exiting Nano

Now that I had saved the file, I pressed `CTRL+X` to exit the nano editor.

### Step 5: Verifying the File

I was back at the main terminal. I checked to make sure our file was saved correctly. I entered `nano cloudworld` to go back into the file using nano and confirmed that everything was correct as the following image shows, and exited the editor.

The file was successfully saved with all the text I entered, demonstrating that nano is a simpler alternative to Vim that allows direct typing without requiring an insert mode.

---

## Summary

Through these exercises, I learned:
- How to use **Vimtutor** to understand Vim basics and navigation
- How to create and edit files using **Vim**, including insert mode, saving, and quitting
- How to use various Vim commands like `dd` (delete line), `u` (undo), and `:w` (save)
- How to use **Nano** as an alternative editor with simpler commands
- The key differences between Vim and Nano editors in terms of workflow and ease of use
