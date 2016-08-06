Modified Ott 0.25
-----------------

Originally, Ott did not allow one to easily rename an inference rule
in Tex.  For example, in an Ott specification one might define a rule
like the following:

```
G, x : A |- t : B
-------------------- :: Fun
G |- \x:A.t : A -> B
```

Unfortunately, Ott does not provide any means of giving a Tex version of
the name "Fun". 

Given the previous rule Ott would generate the following LaTex
(assuming no prefix was given to Ott, and that the judgment prefix is
"T_"):

```
\newcommand{\OttdruleTXXFun}[1]{\Ottdrule[#1]{%
\Ottpremise{\Gamma \Ottsym{,}  \mathit{x}  \Ottsym{:}  \Ottnt{A}  \vdash  \Ottnt{t}  \Ottsym{:}  \Ottnt{B}}%
}{
\Gamma \vdash   \lambda  \mathit{x}  :  \Ottnt{A} . \Ottnt{t}   \Ottsym{:}  \Ottnt{A}  \to  \Ottnt{B}}{%
{\Ottdrulename{T\_Fun}}{}%
}}
```

We can see that the name of the rule "T\_Fun" is not easily changed within LaTex.  

We fix this issue by having Ott factor out the name of each rule into
a constant LaTex command that can be renewed in LaTex.  Now given the
rule above Ott will generate the following:

```
\newcommand{\OttdruleTXXFunName}[0]{\Ottdrulename{T\_Fun}}
\newcommand{\OttdruleTXXFun}[1]{\Ottdrule[#1]{%
\Ottpremise{\Gamma \Ottsym{,}  \mathit{x}  \Ottsym{:}  \Ottnt{A}  \vdash  \Ottnt{t}  \Ottsym{:}  \Ottnt{B}}%
}{
\Gamma \vdash   \lambda  \mathit{x}  :  \Ottnt{A} . \Ottnt{t}   \Ottsym{:}  \Ottnt{A}  \to  \Ottnt{B}}{%
{\OttdruleTXXFunName}{}%
}}
```

Then to rename the rule we can simply add the following after the Ott
generated LaTex include file has been importanted:

```
\renewcommand{\OttdruleTXXFunName}[0]{$\rightarrow_I$}
```

I felt that this modification was the least problematic when it comes
to backward compatibility.  