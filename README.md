#[베이지안 기법, 해커답게 알아보자](http://camdavidsonpilon.github.io/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/)
#### *파이선과 PyMC 를 이용하여 알아보는 베이지안 방법론 학습*


베이지안 기법은 숨겨진 값을 알아내기 (inference) 위해 쓰기 좋은 기법입니다. 
이 분석 방식을 이해하기 위해선 수학적으로 분석하는 능력이 필요한데요 
그래서 사람들이 이런게 있다는 걸 잘 모르기도 하지요. 
베이지안 추론을 다루는 서적을 보자면 일반적으로 두세 챕터 정도를 확률에 대해 가르친 다음 그제서야 베이지안 추론이 뭔지를 가르쳐줍니다. 
근데 베이지안 모델은 그런 방식으로는 설명하기가 힘들어요. 그래서 별 도움 안되는 예제들이나 보여주기 마련입니다. 
이렇게 배우게 되면 *그래서 뭐 어쩌라고* 싶습니다. 뭐 일단 저는 그렇게 생각했어요. 

<div style="float: right; margin-left: 30px;"><img title="created by Stef Gibson at StefGibson.com"style="float: right;margin-left: 30px;" src="http://i.imgur.com/6DKYbPb.png?1" align=right height = 350 /></div>

최근 기계학습 기법 대회에서 베이지안 기법들이 꽤 결과가 좋더라구요. 
그래서 전 해당 기법에 다시 한번 도전해보기로 했죠. 
제가 수학을 좀 하거든요 그런데도 사흘 내내 예제들만 봤어요 그리고 어떻게든 그걸로 기법을 이해해보려고 했죠.
이론을 가지고 어떻게 적용해야 할지 잘 설명해주고 있는 제대로 된 정보가 별로 없더라구요.
베이지안 관련 수식들 (Bayesian mathematics) 와 확률 프로그래밍 (Probablistic programming) 들이 
어떻게 연결되는지에 대해서 전 오해했었고 그 때문에 전 도통 이걸 어떻게 이해해야할지 몰랐죠.
무슨 말이냐면, 제가 그 삽질을 한 덕분에 여러분들은 그럴 필요가 없을 것이다 이 말입니다. 
전 그 부분을 이해하는데 도움이 되고자 해요.

베이지안 기법을 공부하는 것이 우리의 목표라고 할 때, 수학적 분석은 그 목표를 얻는 수단이라 할 수 있습니다. 
그런데, 굳이 수학적 분석 방식대로 공부하지 않고 우회해도 됩니다. 
컴퓨터 자원이 남아도니까 확률 계산용 프로그래밍 (Probabilistic programming) 만 이해하고 그걸 통해서 이해할 수 있습니다.
이게 훨씬 쓸때가 많아요. 왜냐면 수식을 일일히 이해할 필요가 없거든요. 
즉, 우리는 골치아픈 수학적 분석기법들을 굳이 공부할 필요가 없습니다. 
이렇게 공부했을 때 좋은 점이 뭐냐면 전체를 조금씩 나눠 하나씩 이해해갈 수 있습니다. 
수식으로 베이지안 기법을 이해하고자 하면 전체를 한방에 이해하던가 아니면 이해못하던가 둘 중 하나에요. 
보통은 영영 이해못하죠. 
게다가 애초에 수학실력이 없으면 애초에 시작조차 못할 겁니다. 

*베이지안 기법, 해커답게 알아보자*는 베이지안 추론을 소개하는 문서가 되고자 합니다. 
우선, 이해를 먼저하고, 그 다음에, 수식을 알아보는 방식으로 작성해봤습니다. 
물론 입문서니까 깊게는 안들어갈거에요. 중요하니까 다시 말합니다. 이건 입문서에요. 
수학 따윈 마스터해버린 사람이라면 수식을 통해서 이해하는 책들을 보면 됩니다. 
수식으로 뭐 표현하는 거 싫어하는 사람이거나 수학에 별 관심없는 사람이라도 
그냥 베이지안 기법을 공부하고 싶을 뿐인거라면 이런 방식만으로 충분할 거에요. 
재밌기도 할 거구요.

PyMC 를 가지고 프로그래밍할 겁니다. 그 이유는 두 개 정도지 않을까합니다. 
일단은, PyMC 관련해서 딱히 좋은 예제들이나 설명이 없더라구요. 
공식 문서를 보니 베이지안 추론이나 확률 계산용 프로그래밍에 대한 지식이 있어야 쓸 수 있도록 되어 있어요. 
우리는 수학 지식과 상관없이 모든 사용자들이 PyMC 를 사용할 수 있도록 이 책이 도움이 되었으면 해요. 
둘째로, 파이선 (Python) 가지고 만드는 수식 계산 프로그램 중에서 꽤 중요하거나 널리 쓰이는 것들을 보니 PyMC 를 꽤 많이 쓰더군요. 

PyMC 를 설치할려면 NumPy가 일단 필요하고, 경우에 한해서 SciPy가 필요해요. 
그리고 이 책에 나오는 예제들은 PyMC, NumPy, Scipy, Matplotlib 정도면 돌아갈 수 있게 했습니다.
그 정도면 환경을 구성하는데 문제 없을거에요. 


구성
------

해당 프로젝트의 온라인 페이지를 [여기서](http://camdavidsonpilon.github.io/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/) 참조하세요. 예제들도 함께 있어요.

이하의 챕터들은 *nbviewer*로 제공되며 
[nbviewer.ipython.org/](http://nbviewer.ipython.org/) 에서 볼 수 있어요. 
깃헙 클론을 하면 다운로드도 받을 수 있지요!


* [**서장:**](http://nbviewer.ipython.org/urls/raw.github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/master/Prologue/Prologue.ipynb) 
우리가 이 프로젝트를 하는 이유.

* [**Chapter 1: 베이지안 기법을 소개합니다**](http://nbviewer.ipython.org/urls/raw.github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/master/Chapter1_Introduction/Chapter1.ipynb)
    베이지안 통계의 철학과 베이지안 기법을 실사례를 소개합니다. "확률 프로그래밍이란 무엇인가"도 소개하구요. 
    예제 : 
    - 문자메세지 비율을 통해 인간의 행동변화을 추론하기
    
* [**Chapter 2: PyMC 를 좀 더 알아보자**](http://nbviewer.ipython.org/urls/raw.github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/master/Chapter2_MorePyMC/Chapter2.ipynb)
    여러 예제들을 통해 파이선과 PyMC를 이용하여 어떻게 베이지안 기법으로 모델을 만드는지를 알아봅니다.
    어떻게하면 베이지안 모델을 만들 수 있을까요?
    예제 : 
    - 커닝하는 학생들이 얼마나 되는지 알아보기. 거짓말하는거까지 감안해서.
    - 챌린지 우주왕복선 폭발 사고의 확률 계산하기
    
* [**Chapter 3: MCMC 까보기**](http://nbviewer.ipython.org/urls/raw.github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/master/Chapter3_MCMC/Chapter3.ipynb)
    어떻게 MCMC 가 동작하는지에 대해 논의합니다.
    예제 : 
    - 여러 모델을 섞어서 베이지안 기법으로 클러스터링하기 (Bayesian clustering with mixture models)
    
* [**Chapter 4: 알려지지 않은 가장 위대한 정리**](http://nbviewer.ipython.org/urls/raw.github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/master/Chapter4_TheGreatestTheoremNeverTold/Chapter4.ipynb)
    진짜 유용하고, 그리고 위험한, 정리: 다수의 법칙 (The Law of Large Numbers). 
    예제 : 
    - 카글 데이터셋을 알아보자. 그리고 나이브한 분석 (naive analysis) 의 위험성을 알아보자.
    - 레딧에 달린 댓글을 반응이 가장 좋은거부터 가장 인기없는거까지 정렬해보자 (이거 생각보다 어렵다).
    
* [**Chapter 5: 팔 잘리기 vs 다리 잘리기 - 골라보시겠어요?**](http://nbviewer.ipython.org/urls/raw.github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/master/Chapter5_LossFunctions/Chapter5.ipynb)
    평가하는 방법 (loss function) 에 대한 소개와 그게 얼마나 잘 베이지안 기법에서 쓰이는지 알아보기.
    예제 : 
    - 결전 *돈이 최고야* 를 풀어내는 법.
    - 금융, 가장 정확하게 예측해보자.
    - 카글 다크월드 대회 (the Kaggle Dark World's competition) 를 우승한 해법.
    
* [**Chapter 6: *우선*순위 (*prior*-ities) 를 제대로 잡아보자**](http://nbviewer.ipython.org/urls/raw.github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/master/Chapter6_Priorities/Chapter6.ipynb)
    아마도 가장 중요한 챕터. 
    여러 질문에 대해 전문가들의 의견을 들어보았습니다. 
    - 손을 여러 개 가진 무장 강도 문제와 베이지안 무장 강도 문제의 해법.
    - 데이터 샘플 개수와 우선값 (prior) 간의 관계는 무엇이 있을까?
    - 전문가의 사전값을 사용하여 금융에서의 미지의 정보를 예측해내기
    
    또한 분석할때 좀 더 객관적이 될 수 있는 유용한 정보들도 알아보자. 
    사전값 (Prior) 의 문제도 함께 말이다. 
       
* **Chapter X1: 머신러닝에서의 베이지안 기법과 모델 검증하기** 
    데이터를 지나치게 사용하는 상황을 해결하는 방법에 대해서 알아봅시다. 
    거기다가 유명한 머신러닝 기법도 알아볼 거에요. 
    또한 릿지 리그레션과 라소 리그레션에 대해서도 알아볼 것입니다.
    - 카글의 *Don't Overfit* 문제에 대한 팀 살리만의 우승 해법
    
* **Chapter X2: More PyMC Hackery**
    PyMC 의 세부를 자세히 알아봅시다. 
    예제 :
    -  실시간 깃헙 저장소의 별표들과 포크에 대한 분석.


    
**PyMC 에 대해 질문있나요?**
여러분의 모델 방식, 학습 종료시 상황, 혹은 기타 질문들이 있으면 [cross-validated](http://stats.stackexchange.com/), the statistics stack-exchange 에 올려주세요.
    
    
이 책을 사용하는 법
-------

여러분은 이 책을 세 가지 서로 다른 방식으로 읽을 수 있습니다. 제가 가장 추천하는 방식부터 나열해볼께요:

1. 제가 가장 추천하는 방법은 바로 지금 이 디렉토리를 클론하시는 겁니다. 그리고 .ipynb 파일들을 여러분의 컴퓨터로 다운받으세요. IPython 이 설치되어 있다면, 
각각의 챕터들을 브라우저에서 직접 볼 수 있고 *거기다가* 내용 수정 및 코드 수정도 할 수 있습니다 (연습하면서 질문도 해보세요). 이게 제가 추천하는 방법입니다. 
아래와 같은 조건을 갖추는게 약간 거추장스럽긴 하지만요.

    -  IPython v0.13 (or greater) is a requirement to view the ipynb files. It can be downloaded [here](http://ipython.org/). IPython notebooks can be run by `(your-virtualenv) ~/path/to/the/book/Chapter1_Introduction $ ipython notebook`
    -  For Linux users, you should not have a problem installing NumPy, SciPy, Matplotlib and PyMC. For Windows users, check out [pre-compiled versions](http://www.lfd.uci.edu/~gohlke/pythonlibs/) if you have difficulty. 
    -  In the styles/ directory are a number of files (.matplotlirc) that used to make things pretty. These are not only designed for the book, but they offer many improvements over the default settings of matplotlib.

2. 두 번째로, nbviewer.ipython.org 사이트를 통해 보시길 추천합니다. 해당 사이트는 IPython notebooks 를 브라우저에서 볼 수 있는 기능을 제공합니다. ([example](http://nbviewer.ipython.org/urls/raw.github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/master/Chapter1_Introduction/Chapter1_Introduction.ipynb)).
해당 책 내용에 커밋할 때마다 동기적으로 업데이트를 시키고 있어요. 위의 모든 챕터들에 있는 링크들을 타고가시면 됩니다.
 
3. PDF 로 이 책을 읽을 수 있습니다. 제가 이걸 추천하지 않는 이유는 이거는 상호작용을 할 수 없거든요. 
이 방식으로 책을 보시겠다면, [nbconvert](https://github.com/ipython/nbconvert) 을 통해 이용하시면 됩니다.
 

설치 및 설정 방법
----------------


If you would like to run the IPython notebooks locally, (option 1. above), you'll need to install the following:

-  IPython 0.13+ is a requirement to view the ipynb files. It can be downloaded [here](http://ipython.org/ipython-doc/dev/install/index.html) 
- Necessary packages are PyMC 2.2, NumPy, SciPy and Matplotlib.   
   -  For Linux/OSX users, you should not have a problem installing the above, [*except for Matplotlib on OSX*](http://www.penandpants.com/2012/02/24/install-python/).
   -  For Windows users, check out [pre-compiled versions](http://www.lfd.uci.edu/~gohlke/pythonlibs/) if you have difficulty. 
   - also recommended, for data-mining exercises, are [PRAW](https://github.com/praw-dev/praw) and [requests](https://github.com/kennethreitz/requests). 
- New to Python or IPython, and help with the namespaces? Check out [this answer](http://stackoverflow.com/questions/12987624/confusion-between-numpy-scipy-matplotlib-and-pylab). 

-  In the styles/ directory are a number of files that are customized for the notebook. 
These are not only designed for the book, but they offer many improvements over the 
default settings of matplotlib and the IPython notebook. The in notebook style has not been finalized yet.



제작 방식
--------

This book has an unusual development design. The content is open-sourced, meaning anyone can be an author. 
Authors submit content or revisions using the GitHub interface. 

### How to contribute

####What to contribute?

-  The current chapter list is not finalized. If you see something that is missing (MCMC, MAP, Bayesian networks, good prior choices, Potential classes etc.),
feel free to start there. 
-  Cleaning up Python code and making code more PyMC-esque
-  Giving better explanations
-  Spelling/grammar mistakes
-  Suggestions
-  Contributing to the IPython notebook styles


####Commiting

-  All commits are welcome, even if they are minor ;)
-  If you are unfamiliar with Github, you can email me contributions to the email below.

리뷰
-------
*아래 리뷰들은 해학적입니다. 근데 진짜임*

"No, but it looks good" - [John D. Cook](https://twitter.com/JohnDCook/status/359672133695184896)

"I ... read this book ... I like it!" - [Andrew Gelman](http://www.andrewgelman.com/2013/07/21/bayes-related)

"This book is a godsend, and a direct refutation to that 'hmph! you don't know maths, piss off!' school of thought...
The publishing model is so unusual. Not only is it open source but it relies on pull requests from anyone in order to progress the book. This is ingenious and heartening" - [excited Reddit user](http://www.reddit.com/r/Python/comments/1alnal/probabilistic_programming_and_bayesian_methods/)



기여자분들 그리고 감사의 말
---------------------------

아래의 모든 기여자 분들에게 감사드립니다 (시간 순으로 나열하였습니다).

Authors | | | |
--- | --- | --- | ---
[Cameron Davidson-Pilon](http://www.camdp.com) |  [Stef Gibson](http://stefgibson.com) | [Vincent Ohprecio](http://bigsnarf.wordpress.com/) |[Lars Buitinck](https://github.com/larsman)
[Paul Magwene](http://github.com/pmagwene) |  [Matthias Bussonnier](https://github.com/Carreau) | [Jens Rantil](https://github.com/JensRantil) |  [y-p](https://github.com/y-p)
[Ethan Brown](http://www.etano.net/) |  [Jonathan Whitmore](http://jonathanwhitmore.com/) | [Mattia Rigotti](https://github.com/matrig) |  [Colby Lemon](https://github.com/colibius)
[Gustav W Delius](https://github.com/gustavdelius) |  [Matthew Conlen](http://www.mathisonian.com/)  | [Jim Radford](https://github.com/radford) |  [Vannessa Sabino](http://baniverso.com/)
[Thomas Bratt](https://github.com/thomasbratt) |  [Nisan Haramati](https://github.com/nisanharamati) |  [Robert Grant](https://github.com/bgrant) | [Matthew Wampler-Doty](https://github.com/xcthulhu)
[Yaroslav Halchenko](https://github.com/yarikoptic) |  [Alex Garel](https://github.com/alexgarel) | [Oleksandr Lysenko](https://twitter.com/sash_ko) |  [liori](https://github.com/liori)
[ducky427](https://github.com/ducky427) |  [Pablo de Oliveira Castro](https://github.com/pablooliveira) | [sergeyfogelson](https://github.com/sergeyfogelson) |  [Mattia Rigotti](http://neurotheory.columbia.edu/~mrigotti/)
[Matt Bauman](https://github.com/mbauman) | [Andrew Duberstein](http://www.andrewduberstein.com/) | [Carsten Brandt](http://cebe.cc/) |  [Bob Jansen](http://web2docx.com)
 [ugurthemaster](https://github.com/ugurthemaster)   | [William Scott](https://github.com/williamscott)   |  [Min RK](http://twitter.com/minrk)  |  [Bulwersator](https://github.com/Bulwersator)
  [elpres](https://github.com/elpres)  |  [Augusto Hack](https://github.com/hackaugusto)  | [Michael Feldmann](https://github.com/michaf)   | [Youki](https://github.com/Youki)
   [Jens Rantil](http://jensrantil.github.io) |  [Kyle Meyer](http://kyleam.com)  |  [Eric Martin](http://ericmart.in)  | [Inconditus](https://github.com/Inconditus)
 [Kleptine](https://github.com/Kleptine)   |  [Stuart Layton](https://github.com/slayton)  |  [Antonino Ingargiola](https://github.com/tritemio)  |  [vsl9](https://github.com/vsl9)
  [Tom Christie](https://github.com/tom-christie)  |  [bclow](https://github.com/bclow)  |  [Simon Potter](http://sjp.co.nz/)  | [Garth Snyder](https://github.com/GarthSnyder)
 [Daniel Beauchamp](http://twitter.com/pushmatrix)  |  [Philipp Singer](http://www.philippsinger.info)  | [gbenmartin](https://github.com/gbenmartin) | [Peadar Coyle](https://twitter.com/Springcoil)

We would like to thank the Python community for building an amazing architecture. We would like to thank the 
statistics community for building an amazing architecture. 

Similarly, the book is only possible because of the [PyMC](http://github.com/pymc-devs/pymc) library. A big thanks to the core devs of PyMC: Chris Fonnesbeck, Anand Patil, David Huard and John Salvatier.

One final thanks. This book was generated by IPython Notebook, a wonderful tool for developing in Python. We thank the IPython 
community for developing the Notebook interface. All IPython notebook files are available for download on the GitHub repository. 



#### 어떻게 연락하나요
Contact the main author, Cam Davidson-Pilon at cam.davidson.pilon@gmail.com or [@cmrndp](https://twitter.com/cmrn_dp)


![Imgur](http://i.imgur.com/Zb79QZb.png)
