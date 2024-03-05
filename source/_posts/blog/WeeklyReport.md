---
title: A Lightweight Designed Beamer Template of Weekly Survey
date: 2024-02-18 17:12:55
toc: true
tags:
    - Project Habits
categories: blog
excerpt: I'd like to share a lightweight designed beamer template of weekly survey with the version of Xidian University, which is useful for reporting your research work academically and concisely.
---
## Introduction
Nowadays, I'm working on a weekly report for my research group. Finding a concise and academic slides template is a need for us to represent our finds and ideas. Based on the [PKU_beamer_lightweight_designed](https://github.com/tongpf/PKU_beamer_lightweight_designed), I adjust some details according to my preference and share the tutorial in this post.

## Adjusted Details
This a lightweight designed beamer template of weekly survey with the version of Xidian University, which is useful for reporting your research work academically and concisely.
+ Theme Color
I use the official red color of Xidian University as the theme color. The hex code of the color is `#B0252A`.
+ Main Font
I prefer the fontstyle of `Times New Roman` so I need to additionally include `fontspec` package.
```latex
\usepackage{fontspec}
\setsansfont{Times New Roman}
``` 
+ Remove Specific Frame From the Healine
In beamer, the default frames will be counted in the headline which is sometimes not suitable. Such as the `title page`, `overline page` and so on are usually independent pages.

To remove them from the headline, I use the following code.
```latex configuration
\makeatletter
\let\beamer@writeslidentry@miniframeson=\beamer@writeslidentry%
\def\beamer@writeslidentry@miniframesoff{%
  \expandafter\beamer@ifempty\expandafter{\beamer@framestartpage}{}% does not happen normally
  {%else
    % removed \addtocontents commands
    \clearpage\beamer@notesactions%
  }
}
\newcommand*{\miniframeson}{\let\beamer@writeslidentry=\beamer@writeslidentry@miniframeson}
\newcommand*{\miniframesoff}{\let\beamer@writeslidentry=\beamer@writeslidentry@miniframesoff}
\makeatother
```
Before those frames, we just turn off the frames style but if we next to count frame we also need to open this by the command of `\miniframeson`. And the `section*{}` ensures the frame removes the number in some section.

```latex usage
\miniframesoff
\begin{frame}
    \section*{}
    \begin{center}
        {\Huge \textit{Thanks for you3fr listening!}}
    \end{center}
\end{frame}
```

### XidianU.sty Codes
```latex XidianU.sty
\mode<presentation>

\newif\ifbeamer@secheader
\beamer@secheaderfalse

\ProcessOptionsBeamer

\useoutertheme[subsection=false]{smoothbars}
\makeatletter
\newcommand{\frameofframes}{/}
\newcommand{\setframeofframes}[1]{\renewcommand{\frameofframes}{#1}}
\setbeamertemplate{footline} 
  {%
    \begin{beamercolorbox}[colsep=1.5pt]{upper separation line foot}
    \end{beamercolorbox}
    \begin{beamercolorbox}[ht=2.5ex,dp=1ex,%
      leftskip=.3cm,rightskip=.3cm plus1fil]{author in head/foot}%
      {\usebeamerfont{author in head/foot}\insertshortauthor}%
      \hfill%
      {\usebeamerfont{title in head/foot}\insertshorttitle}%
      \hfill%
      {\usebeamerfont{frame number}\usebeamercolor[fg]{frame number}\insertframenumber~\frameofframes~\inserttotalframenumber}
    \end{beamercolorbox}%
    \begin{beamercolorbox}[colsep=1.5pt]{lower separation line foot}
    \end{beamercolorbox}
  }
\makeatother

\useinnertheme{circles}
%\useinnertheme{rectangles}
%\useoutertheme{default}
%\useinnertheme[shadow=true]{rounded}

\definecolor{xidian}{HTML}{B0252A}
% \xdefinecolor{xidian}{cmyk}{0,1,1,0.45}%{rgb}{0.543,0.0,0.0703} %{cmyk}{0,100,100,45}%{rgb}{0.5,0.0,0.0}  %RGB#820010
\xdefinecolor{xidian_gold}{cmyk}{0,0.35,0.75,0.05}
\xdefinecolor{xidian_blue}{cmyk}{0.6,0.35,0.0,0.4}
\xdefinecolor{xidian_darkblue}{cmyk}{1.0,0.6,0.0,0.5}
\xdefinecolor{xidian_gray}{cmyk}{0.0,0.0,0.08,0.55}
\xdefinecolor{xidian_dirt}{cmyk}{0.0,0.2,0.35,0.3}
\xdefinecolor{xidian_orange}{cmyk}{0.0,0.7,1.0,0.0}
\xdefinecolor{xidian_green}{cmyk}{0.2,0.0,1.0,0.15}
\xdefinecolor{xidian_darkgreen}{cmyk}{0.6,0.5,1.0,0.45}
\xdefinecolor{pantone_gold}{RGB}{135,103,79}
\xdefinecolor{pantone_silver}{RGB}{138,141,143}
\xdefinecolor{WM_Gold}{cmyk}{0.09,0.29,0.66,0.24}
\setbeamercolor{footline}{bg=xidian}
%\setbeamercolor{frametitle}{bg=white!70!pantone_gold,fg=xidian}
\setbeamercolor{frametitle}{bg=white,fg=xidian}
\setbeamercolor{title}{bg=xidian}
%\setbeamerfont{frametitle}{size=\large}
\setbeamerfont{frametitle}{series=\bfseries,size=\large}%,parent=structure}
\setbeamerfont{footline}{series=\bfseries}

\setbeamertemplate{navigation symbols}{}
\setbeamertemplate{bibliography item}[text]
\setbeamertemplate{caption}[numbered]

\beamertemplateshadingbackground{white!5}{white}

\setbeamercolor{palette primary}{use=structure,fg=white,bg=structure.fg}
\setbeamercolor{palette secondary}{use=structure,fg=white,bg=structure.fg!95!black}%{use=structure,fg=white,bg=structure.fg!90!black}
\setbeamercolor{palette tertiary}{use=structure,fg=white,bg=structure.fg!90!black}
\setbeamercolor{palette quaternary}{fg=white,bg=structure.fg!85!black}
%\setbeamercolor*{sidebar}{use=structure,bg=structure.fg}
\setbeamercolor{titlelike}{parent=palette primary}

%% try
\setbeamercolor{block title}{bg=xidian_blue,fg=white}
\setbeamercolor{block body}{bg=xidian_blue!10}

\BeforeBeginEnvironment{definition}{%
    \setbeamercolor{block title}{bg=xidian_blue,fg=white}
    \setbeamercolor{block body}{bg=xidian_blue!10}
}
\AfterEndEnvironment{definition}{
 \setbeamercolor{block title}{bg=xidian_blue,fg=white}
 \setbeamercolor{block body}{bg=xidian_blue!10}
}

\BeforeBeginEnvironment{theorem}{%
    \setbeamercolor{block title}{bg=xidian_orange,fg=white}
    \setbeamercolor{block body}{bg=xidian_orange!10}
}
\AfterEndEnvironment{theorem}{
 \setbeamercolor{block title}{bg=xidian_blue,fg=white}
 \setbeamercolor{block body}{bg=xidian_blue!10}
}

\BeforeBeginEnvironment{proposition}{%
    \setbeamercolor{block title}{bg=xidian_orange,fg=white}
    \setbeamercolor{block body}{bg=xidian_orange!10}
}
\AfterEndEnvironment{proposition}{
 \setbeamercolor{block title}{bg=xidian_blue,fg=white}
 \setbeamercolor{block body}{bg=xidian_blue!10}
}

\setbeamercolor*{block title example}{use={normal text,example text},bg=white!70!pantone_gold,fg=xidian}
\setbeamercolor{fine separation line}{}
\setbeamercolor{item projected}{fg=white}
\setbeamercolor{palette sidebar primary}{use=normal text,fg=normal text.fg}
\setbeamercolor{palette sidebar quaternary}{use=structure,fg=structure.fg}
\setbeamercolor{palette sidebar secondary}{use=structure,fg=structure.fg}
\setbeamercolor{palette sidebar tertiary}{use=normal text,fg=normal text.fg}
%\setbeamercolor{palette sidebar quaternary}{fg=white}
\setbeamercolor{section in sidebar}{fg=brown}
\setbeamercolor{section in sidebar shaded}{fg=grey}
\setbeamercolor{separation line}{}
\setbeamercolor{sidebar}{bg=xidian}
\setbeamercolor{sidebar}{parent=palette primary}
\setbeamercolor{structure}{fg=xidian}
\setbeamercolor{subsection in sidebar}{fg=brown}
\setbeamercolor{subsection in sidebar shaded}{fg=grey}

\AtBeginSection[]{
	\begin{frame}
		\tableofcontents[sectionstyle=show/shaded,subsectionstyle=hide,subsubsectionstyle=hide]
	\end{frame}
}  


\setbeamercolor{postgreen}{fg=black,bg=example text.fg!75!black!10!bg}
\setbeamercolor{postred}{fg=black,bg=white!70!pantone_gold}
\setbeamercolor{postblue}{fg=black,bg=xidian_blue!10}
%\AtBeginSubsection[]{
%	\begin{frame}
%		\tableofcontents[sectionstyle=show/shaded,subsectionstyle=hide,subsubsectionstyle=hide]
%	\end{frame}
%}

\mode
<all>
```
### main.tex
```latex main.tex
\documentclass[10pt,hyperref={colorlinks,citecolor=blue,urlcolor=xidian_blue,linkcolor=}]{beamer}
\usepackage{XidianU}
\usepackage{fontspec}
\setsansfont{Times New Roman}

\usepackage{lipsum}
%\usepackage[scheme = plain]{ctex}
\usepackage{charter} % Nicer fonts
% other packages
\usepackage{latexsym,amsmath,xcolor,multicol,booktabs,calligra}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{subcaption}
\usepackage{bm}
\usepackage{natbib}
\usepackage{wrapfig}
\usepackage{amsfonts} 
\usepackage{ragged2e}
\usepackage{parskip}

\apptocmd{\frame}{}{\justifying}{} % Allow optional arguments after frame.
\newcommand{\theHalgorithm}{\arabic{algorithm}}
\theoremstyle{plain}
\newtheorem{axiom}{Axiom}
\newtheorem{claim}[axiom]{Claim}
\newtheorem{assumption}{Assumption}
\newtheorem{remark}{Remark}
\newtheorem{proposition}{Proposition}
\setbeamertemplate{theorems}[numbered]

% change for your title page information
\author[Zhihao Li]{Zhihao Li}
\title{Research Survey 1}
\subtitle{Why Is Prompt Tuning for Vision-Language Models Robust to Noisy Labels?}
\institute{School of Computer Science and Technology\\Xidian University}
\date{February 1, 2024}

% official colors match with the Xidian color
\def\cmd#1{\texttt{\color{red}\footnotesize $\backslash$#1}}
\def\env#1{\texttt{\color{blue}\footnotesize #1}}
\definecolor{deepblue}{rgb}{0,0,0.5}
\definecolor{deepred}{rgb}{0.6,0,0}
\definecolor{deepgreen}{rgb}{0,0.5,0}
\definecolor{halfgray}{gray}{0.55}

\show\hss

\makeatletter
\let\beamer@writeslidentry@miniframeson=\beamer@writeslidentry%
\def\beamer@writeslidentry@miniframesoff{%
  \expandafter\beamer@ifempty\expandafter{\beamer@framestartpage}{}% does not happen normally
  {%else
    % removed \addtocontents commands
    \clearpage\beamer@notesactions%
  }
}
\newcommand*{\miniframeson}{\let\beamer@writeslidentry=\beamer@writeslidentry@miniframeson}
\newcommand*{\miniframesoff}{\let\beamer@writeslidentry=\beamer@writeslidentry@miniframesoff}
\makeatother

\begin{document}
{
\begin{frame}
    \titlepage
    \begin{figure}[htpb]
        \begin{center}
            \includegraphics[width=0.2\linewidth]{Figures/XDUlogo.jpg}
        \end{center}
    \end{figure}
\end{frame}
}
\section{Summary}
\begin{frame}{Weekly Work}
\begin{enumerate}
    \item Read the paper of \textit{Why Is Prompt Tuning for Vision-Language Models Robust to Noisy Labels?};
    \item Learn about some concepts;
\end{enumerate}
\end{frame}

\begin{frame}{A prompt tuning process is highly
robust to label noises.}


\begin{enumerate}
    \item \textbf{Interest}: Studying the key reasons contributing to the robustness of the prompt tuning.
paradigm.
    \item \textbf{Findings}: \begin{enumerate}
        \item the fixed classname tokens provide a strong regularization to the optimization of the model, reducing gradients induced by the noisy samples;
        \item the powerful pre-trained image-text embedding that is learned from diverse and generic web data provides strong prior knowledge for image classification.
    \end{enumerate}
\end{enumerate}
\end{frame}

\begin{frame}{Author's Contributions}
    \begin{itemize}
        \item We demonstrate that \textbf{prompt tuning for pre-trained vision-language models (e.g., CLIP) is more robust to noisy labels} than traditional transfer learning approaches, such as model fine-tuning and linear probes.
        \item We further demonstrate that \textbf{prompt tuning robustness can be further enhanced through the use of a robust training objective.}
        \item We conduct an extensive analysis on why prompt tuning is robust to noisy labels to \textbf{discover which components contribute the most to its robustness.}
        \item We \textbf{propose a simple yet effective method for unsupervised prompt tuning}, showing that randomly selected noisy pseudo labels can be effectively used to enhance CLIP zero-shot performance. The proposed robust prompt tuning outperformed prior work on a variety of datasets, even though noisier pseudo-labels are used for self-training.
    \end{itemize}
\end{frame}

\section{Motivations}
\begin{frame}{Mathematical Models}
    \begin{itemize}
        \item CLIP\\
        In the case of image classification, a normalized image embedding $\boldsymbol{f}^{\:v}$ is obtained by passing an image through CLIP's visual encoder, and a set of normalized class embeddings $[\boldsymbol{f_i^{\:t}}]_{i=1}^K$ by feeding template prompts of the form "A photo of a" into CLIP's text encoder.
        \begin{equation}
        Pr(y=i|\boldsymbol{x})=\frac{\exp(sim(\boldsymbol{f}^{\:v},\boldsymbol{f}^{\:t}_i))/\tau}{\sum_{j=1}^K\exp(sim(\boldsymbol{f}^{\:v},\boldsymbol{f}^{\:t}_j))/\tau}
        \end{equation}
        \item Prompt Tuning\\
        The name of a class c is first converted into a classname embedding $\boldsymbol{w}\in R^d$ and prepended with a sequence of $M$ learnable tokens $\boldsymbol{p_m}\in R^d$ shared across all classes.
        \begin{equation}
        P_c=[\boldsymbol{p_1}, \boldsymbol{p_2}, \cdots, \boldsymbol{p_M}, \boldsymbol{w_c}]\rightarrow \boldsymbol{f}^{\:t}_c
        \end{equation}
        CoOp optimizes the shared learnable tokens $\boldsymbol{p_1}, \boldsymbol{p_1}, \cdots, \boldsymbol{p_M}$ on a small labeled dataset $D = [(\boldsymbol{x_i}, c_i)^N_{i=1}]$ to minimize the cross-entropy loss:
        \begin{equation}
            L_{CE}=-E_{(\boldsymbol{x},c)\in D}[\log Pr(y=c|\boldsymbol{x})]
        \end{equation}
    \end{itemize}
\end{frame}

\begin{frame}{Mathematical Models}
    \begin{itemize}
        \item Robust Prompt Tuning\\
        Further enhance this robustness by optimizing the learnable prompts using the generalized cross-entropy (GCE) loss:
        \begin{equation}
            L_{GCE}=E_{(\boldsymbol{x},c)\in D}[\frac{1-Pr(y=c|\boldsymbol{x})^q}{q}]
        \end{equation}
        \item Author's Conclusion: $q = 0.7$ leads to overall good performance across several experimental settings.
    \end{itemize}
\end{frame}

\section{Robustness Analysis}

\begin{frame}{Pre-trained CLIP Generates Effective Class Embeddings}
    \vspace{-1em}
    \begin{figure}
    \includegraphics[width=0.9\textwidth]{Figures/Survey1/models.png}
    \label{fig: Models}
    \end{figure}\vspace{-0.7em}
    \begin{itemize}
        \item Classifier-R v.s. Classifier-C: CLIP class embeddings provide a strong initialization for few-shot learning.\vspace{-0.5em}
        \item TEnc-FT v.s. Classifier-C: The highly expressive CLIP text encoder can easily overfit to the noisy labels.\vspace{-0.5em}
        \item Prompt Tuning v.s. Classifiers: The text encoder is essential for providing a strong but informative regularization of the text embeddings to combat noisy inputs.\vspace{-0.5em}
        \item Prompt Tuning v.s. TEnc-FT: The text encoder should be fixed to prevent overfitting.
    \end{itemize}
\end{frame}
\begin{frame}{Other Aspects of Robustness}
\begin{itemize}
    \item \textbf{Effectiveness of Prompt}
    \item \textbf{Prompt Tuning Suppresses Noisy Gradients}
    \item \textbf{Generalization Across Model Architectures}
    \item \textbf{Robustness to Correlated Label Noise}
\end{itemize}
\end{frame}

\section{Robust UPL}
\begin{frame}{Improve UPL in Unsupervised Prompt Tuning}
    \vspace{-1em}
    \begin{figure}
    \includegraphics[width=\textwidth]{Figures/Survey1/UPL.png}
    \label{fig: UPL}
    \end{figure}\vspace{-0.8em}
\begin{itemize}
    \item Baseline UPL\begin{itemize}
        \item Phase 1: Leverage pre-trained CLIP to generate pseudo labels for unlabeled images.
        \item Phase 2: Select \textbf{the $K$ most confident samples per class} to optimize the learnable tokens through the typical prompt-tuning optimization process (described in CoOp).
        \end{itemize}
    \item Robust UPL\\
    Based on UPL, \textbf{randomly sample $K$ training samples} and optimize the prompt with the \textbf{robust GCE loss}.
\end{itemize}
\end{frame}
\section{Next Stage}
\begin{frame}{New Plans for Next Week}
\begin{enumerate}
    \item Reproduce the most of results about this paper.
    \item Survey other relavent methods in this domain.
\end{enumerate} 
\end{frame}

\miniframesoff
\begin{frame}
    \section*{}
    \begin{center}
        {\Huge \textit{Thanks for you3fr listening!}}
    \end{center}
\end{frame}

\end{document}
```
## Template Overview
<iframe src="/pdfjs/web/viewer.html?file=/pdf/papers/WeeklyReportTemplate.pdf" style='width:100%;height:800px'></iframe>

## Contributors
+ [Zhihao Li](https://lzhms.github.io/)

## References
+ [PKU_beamer_lightweight_designed](https://github.com/tongpf/PKU_beamer_lightweight_designed)