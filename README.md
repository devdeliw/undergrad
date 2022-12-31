# My approach to taking notes
I take lecture notes using Vim, VimTex, Skim, and UltiSnips. 

- **Vim** is a popular IDE for TeX progammers. 

- **VimTeX** is a Vim plugin to read and compile TeX code from Vim and convert into PDF format. 

- **Skim** is a PDF Viewer for MacOS. 

- **UtilSnips** is a way to write short-hand TeX (i.e. write your notes much faster). Note: this requires python.

  

## Install VimTeX

This tutorial assumes that a working version of Vim has been installed already. Vim can be downloaded [here](https://www.vim.org/download.php).

Before installing VimTeX, install **vim-plug manager** using [these instructions](https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim). 

Next locate or create the `.vimrc` in your home directory. See an [example](https://gist.github.com/simonista/8703722) of a typical .vimrc file.

Next, to install the VimTeX plugin add the following code snippet inside the .vimrc file: 

```bash
call plug#begin()
	Plug 'lervag/vimtex'
call plug#end()
```

To install any other plugins using vim-plug, the `Plug `statement has to be within the `call plug#begin()...call plug#end()` environment as well. 

Finally, open vim, and type in command mode `:PlugInstall` to complete the VimTex installation.

Though it is possible to open the PDF file in one of the many commercially available PDF readers, a seamless integration with Vim (in our case) is appreciated. For Windows and Linux  users, _Zathura_ is the PDF viewer that works really well with VimTex. For Unix (MacOS) users, _Skim_ is the viewer that works.

## Install Skim

This tutorial explains how to install Skim as the PDF viewer for VimTeX. For Windows users, a tutorial on how to sync Zathura with VimTeX can easily be found online. However, Skim also works with Windows.

To install Skim, run in terminal:

```bash
$ brew install --cask skim
```

## Connect VimTeX and Skim

In the `.vimrc` file, add the following code snippets to enable VimTeX to talk to Skim from within Vim and display the output PDF.

Identify the default TeX file format:

```matlab
let g:tex_flavor='latex'
```

Enable the program to view the PDF output via skim:

```matlab
let g:vimtex_view_method = 'skim'
```

Enable _forward search_ to allow any change made in TeX to automatically refresh Skim and reflect those exact changes in PDF after every save and successful compilation. 

```matlab
let g:vimtex_view_skim_sync = 1
```

Enable _change focus_ to allow VimTex to place the cursor at the position in the PDF corresponding to the same TeX code in the TeX file after successful completion. This is particularly useful when troubleshooting.

```matlab
let g:vimtex_view_skim_activate = 1
```

To enable _backward search_, where the PDF shows its corresponding _code_ in the TeX program, we need to change Skim's settings. 

In the **preferences** page of Skim, navigate to the **Sync** Tab, and under the section labeled **PDF-TeX Sync Support,** make **preset** as **custom**,  **"command"** as **vim**, and set **arguments** as the following:

```bash
--headless -c "VimtexInverseSearch %line '%file'"
```

With just four lines of settings in the .vimrc file and one line in the Skim preferences page, we activated both forward and backward search between VimTeX and Skim. Now TeX will work through Vim using both VimTeX and Skim. 

## Write TeX faster

The one draw back of TeX is having to repeatedly write simple commands like `\begin{}...\end{}`. This can be tedious and cause compilation errors.

To work around this, programmers use UltiSnips to pre-program "snippets," i.e. shorthand text to autofill common commands in TeX. For example, using UltiSnips, simply typing `beg` and clicking ⇥ (tab key), could autofill with the following:

```TeX
\begin{|}

\end{}
```

and have the cursor inside the `{}` block to quickly continue the writing. Clicking ⇥ (tab) again will move the cursor directly inside the environment for simpler and quicker programming.

## Install UltiSnips 

UltiSnips requires a working installation of Python 3. In addition, Vim must be compiled with the python3 feature enabled - you can test this with `:echo has("python3")`, which will return `1` if it is enabled and `0` if it isnt.

Using vim-plug, add a Plug for UltiSnips:

```bash
call plug#begin()
Plug 'lervag/vimtex'
Plug 'sirver/ultisnips'
call plug#end()
```

Next, in vim, run `:PlugInstall` to finish installing UltiSnips.

### Developing snippets

Small programming in UltiSnips is required to add snippets.

In Vim, open any TeX file, and in command mode type `:UltiSnipsEdit` to open a separate document where snippets can be programmed.

In UltiSnips, to program shorthand commands, use the following tempate: 
```bash
snippet <shorthand command> "Program Name" <custom configurations>
<"command to replace">
endsnippet
```

For example, to short hand the TeX command: `\vspace{20px}`, using the shorthand command `\v` + ⇥ (tab), the code in UltiSnips would be:

```TeX
snippet \v "Vspace Command"
\vspace{20px}
endsnippet
```

We may want to write code to allow us to actually _type_ in these code snippets as well; for example writing different `\begin{}...\end{}`, or inserting an image using `\includegraphics{<image.png>}`.

To do this, UltiSnips allows user to insert certain `$1,$2,...,$n` commands into the code snippets to tell TeX where the cursor should go after pressing ⇥ n times.

For example, the snippet

```Tex	
snippet beg "Begin/End"
\begin{$1}
	$2
\end{$1}
$3
```

allows the user to type "beg" + ⇥ to generate the `\begin{}..\end{}` environment skeleton. In addition, the `$1` location is where the cursor will _first_ go, allowing a user to quickly specify the environment. Since both `\begin{$1}` and `\end{$1}` have `$1`, both `{}` are filled at the same time. After the environment is defined, pressing ⇥, places the cursor at position `$2`; pressing the tab again places the cursor at `$3`, and so on. 

There is also a `[custom configurations]` in the UltiSnips snippet template. Custom configurations allow for autocompleting snippets without having to press ⇥, and manyother useful features.

For example, the same snippet

```TeX
snippet beg "Begin/End" iA
\begin{$1}
	$2
\end{$1}
$3
```

but instead with an `iA` beside the `"Begin/End"` snippet name allows the user to simply type "beg," to immediately autocomplete the snippet without having to press ⇥. 

Every custom configuration is outlined in the UltiSnips author's [github](https://github.com/SirVer/ultisnips). 

Now, making "beg" instantly autocomplete to an entire environment skeleton is not very wise. To learn how to use UltiSnips snippets effectively, I suggest creating and growing **your own** personal snippet keybinds as you progress in TeX, rather than trying to learn someone else's keybinds from online. 

Now you should have a fast and effective way to write and compile TeX using Vim, VimTeX, Skim, and UltiSnips!

This note-taking setup was heavily inspired by Gilles Castel's [article](https://castel.dev/post/lecture-notes-1/). He provides many examples of useful snippets for mathematics note-taking, and a way to include math figures seamlessly. 
