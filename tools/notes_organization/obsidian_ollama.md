# Obsidian with Ollama

## Used tools

 - [Obsidian](https://obsidian.md/)
 - [Ollama](https://github.com/jmorganca/ollama)
 - [Obsidian Ollama plugin](https://github.com/hinterdupfinger/obsidian-ollama)

## Why

For me, Obsidian is a multiplatform closed source glorified electron based markdown text editor with abhorrent vimmotions/commands integration and decent plugin ecosystem

As I currently lack knowledge of a better alternative and don't have the motivation to prepare a [neovim](https://github.com/neovim/neovim) setup that would fit my needs for this usecase, I am temporarily using it for thoughts recording and notes storage

But I have a problem, organization problem

When I write down my thoughts, there is no time for organization  
I have to write what I think as fast as possible, wherever my cursor is, words are written

When I read those thoughts, it was hard to see how they related to each other  
So, I started organizing them, but found that to be quite annoying

At first I thought that maybe the process of organizing would help me understand everything better  
But alas, turned out it wasn't helpful by itself, only the results were

And thus I had a thought, why not delegate that work onto AI?

## Setup

1. Install [the tools](#used-tools)
2. Run ollama

```sh
ollama serve
```

3. Download chosen model, in my case, 7B parameter Mistral model finetuned on Openorca dataset with "type 1" 4-bit quantization of M variant

```sh
ollama run mistral-openorca:7b-q4_K_M
```

4. Setup ollama obsidian plugin  
Here we have to think of a prompt to send to the model, this is extremely important, and requires experimentation to get a result that you are happy with

## Results

As a way of showing by example, this section will contain comparison of how 2 different models, previously discussed mistral openorca and the base foundational llama2, handled the task with prompt set to

```
Rewrite the given text by separating it into sections and ordering the content in a way that a reader who only knows the previous sections would understand the text. Make sure the meaning is preserved without any changes. The sections shouldn't be short unless the topic is unlike any other section and requires a section of its own. Use Markdown formatting.
```

### Database handling note

#### Before

```markdown
️user can choose export/import location, external files folder by default  
user can export database to set or chosen location  
user can import database from set or chosen location  
user can choose to have room use external files folder db (ask on first startup? also check if possible to use other locations if there is a saved db file there, if that's possible we can technically create the file in internal and then copy it to chosen location, set the db location to that file and then restart the app)
```

#### After (mistral-openorca:7b-q4_K_M)

```markdown
# Exporting Database

User can choose an export/import location. By default, the external files folder is used. The user has the option to export the database to a set or chosen location.

# Importing Database

The user can import the database from a set or chosen location. This allows for easy transfer of data between devices and storage locations.

# Using External Files Folder for Database

When using an external files folder for the database, the user has the option to have the room use this folder as the default location for the database. This can be set during the first startup or changed later if needed. Additionally, if there is a saved database file in another location, it is possible to create the file in the internal storage and then copy it to the chosen location. Once this is done, the database location can be set to that file, and the app can be restarted.️
```

#### After (llama2)

```markdown
️ Section 1: User Can Choose Export/Import Location
---------------------------------------------

* The user can choose the location for exporting or importing a database.
* By default, the external files folder will be used.

️ Section 2: User Can Export Database to Set or Chosen Location
--------------------------------------------------------

* The user can export the database to a set or chosen location.
* This is optional and can be done at any time.

️ Section 3: User Can Import Database from Set or Chosen Location
-------------------------------------------------------------

* The user can import a database from a set or chosen location.
* This is also optional and can be done at any time.

️ Section 4: User Can Choose to Use External Files Folder DB (Ask on First Startup?)
-----------------------------------------------------------------------

* The user can choose to have the room use an external files folder for the database.
* This will be asked during the first startup of the app.
* If possible, the app will check if there is a saved db file in other locations and offer to use those locations instead.
* If a file is created in the internal location, it will be copied to the chosen location and the db location will be set to that file before restarting the app.️
```

### Obsidian Ollama note

#### Before

```markdown
slight changes in the prompt can change the result significantly, results can vary from situation of having a section with a singular line of text just like clean coders like to keep their files, to gigachad everything in 1 section, which in this case is like completely ignoring the prompt

sometimes adds text of its own, or changes the text even if told not to

no real time typing in current implementation, which makes it impossible to know what's happening until we get the result, which if the model starts randomly repeating the same thing over and over as it sometimes does, might take a _while_
```

#### After (mistral-openorca:7b-q4_K_M)

```markdown
# Section 1: Understanding the Prompt

Slight changes in the prompt can change the result significantly. Results can vary from having a section with a singular line of text, just like clean coders like to keep their files, to gigachad everything in one section, which is like completely ignoring the prompt.

# Section 2: Text Manipulation

Sometimes, the AI adds text of its own or changes the text even if told not to. This can lead to unexpected results and make it difficult to understand the original intent.

# Section 3: Real-Time Typing

Currently, there is no real-time typing in the implementation. This means that we cannot see what's happening as it occurs, and we only get the result after waiting for the model to finish processing. If the model starts randomly repeating the same thing over and over, as it sometimes does, this can take a while.️
```

#### After (llama2)

```markdown
### Section 1: Introduction

* Briefly explain the task of rewriting text by separating it into sections and ordering the content in a way that a reader who only knows the previous sections would understand the text.
* Mention that slight changes in the prompt can change the result significantly, and results can vary from situation to situation.

### Section 2: Importance of Organization

* Emphasize the importance of organizing the text into sections, especially when working with long texts or complex topics.
* Explain how separating the text into sections helps readers navigate the content more easily and understand the main ideas without getting overwhelmed by too much information at once.

### Section 3: Challenges of Rewriting Text

* Discuss the challenges of rewriting text, such as dealing with repetitive or irrelevant information, and ensuring that the meaning of the text is preserved without any changes.
* Mention that sometimes the model may add text of its own or change the text even if told not to, which can affect the outcome of the rewrite.

### Section 4: Current Implementation

* Describe the current implementation of rewriting text, including the fact that there is no real-time typing and that it can take a while to get the result.
* Mention that if the model starts randomly repeating the same thing over and over as it sometimes does, it may take even longer to get the desired outcome.

### Section 5: Conclusion

* Summarize the main points of the article and emphasize the importance of organizing text into sections for easy navigation and understanding.
* Encourage readers to try rewriting text in a way that makes sense for their specific use case, taking into account any challenges or limitations they may face.️
```

## Issues

### Model

As with any AI model, slight changes in the prompt can change the result significanty  
Results can vary from singular line of text per section, as if inspired by [Clean Code](https://github.com/dev-marko/clean-code-book/blob/master/Clean%20Code%20(%20PDFDrive.com%20).pdf) practitioners keeping barely more than a single line of code in each file, to gigachad everything in 1 section, which, well, in this case is like completely ignoring the prompt (and isn't exactly desirable in code either)

Another issue I have run into was the model adding or changing the text, even with a prompt that explicitly said not to do that  
However, fortunately, I have not noticed it blatantly removing any text

Finally, the biggest issue in my opinion, which we can see in [obsidian ollama note result](#after-mistral-openorca7b-q4_k_m-1), is the dehumanization of the notes, AI removes any traces that could indicate that this was written by a human  
sarcasm? removed  
text formatting that could have been used to indicate emotions? removed  
making this a corporate dream come true tool  
This issue might be fixable by prompt experimentation, but I wasn't able to do that yet

### Obsidian plugin

This section is based on version [0.0.1](https://github.com/hinterdupfinger/obsidian-ollama/releases/tag/0.0.1) of the plugin

#### No real time typing

[Github issue](https://github.com/hinterdupfinger/obsidian-ollama/issues/16)

The plugin doesn't incorporate real time typing, which makes it impossible to know what is happening until the model stops generating text, which, if the model starts randomly repeating the same thing over and over, as it sometimes does, might take a _**while**_

#### Settings

The plugin settings aren't very user friendly, not only do they [not save](https://github.com/hinterdupfinger/obsidian-ollama/issues/2), which, specifically for the model input, forces the user to write the same thing over and over, once added as a command, the user can neither edit, nor copy them  
The only way to edit a command without deleting and creating a new one is to directly edit the obsidian plugin json file