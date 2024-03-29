<h3 align = "center"> PACE: Program Analysis Framework for Continuous Performance Prediction </h3>

---


<p align="center"> <img src="..doc/pace1.jpeg" width="95%"> </p>

Official implementation of ```PACE: Program Analysis Framework for Continuous Performance Prediction```   ACM Transactions on Software Engineering and Methodology (TOSEM) 2023.

## Abstract
> Software development teams establish elaborate continuous integration pipelines containing automated test cases to accelerate the development process of software. Automated tests help to verify the correctness of code modifications decreasing the response time to changing requirements. However, when the software teams do not track the performance impact of pending modifications, they may need to spend considerable time refactoring existing code. This paper presents PACE, a program analysis framework that provides continuous feedback on the performance impact of pending code updates. We design performance microbenchmarks by mapping the execution time of functional test cases given a code update. We map microbenchmarks to code stylometry features and feed them to predictors for performance predictions. Our experiments achieved significant performance in predicting code performance, outperforming current state-of-the-art by 75\% on neural-represented code stylometry features.
<hr>


Cite:
```
@article{biringa2023pace,
  title={PACE: A Program Analysis Framework for Continuous Performance Prediction},
  author={Biringa, Chidera and Kul, G{\"o}khan},
  journal={ACM Transactions on Software Engineering and Methodology},
  year={2023},
  publisher={ACM New York, NY}
}
```

<hr>
Artifact Author: Chidera Biringa
<hr>

## Installation
```
$ git clone https://github.com/biringaChi/PACE 
$ pip install -r requirements.txt 
$ cd src
```

## Problem Definition
<p align="center"> <img src="..doc/problem.svg" width="60%"> </p>

> Figure 1: Access and manipulation of a Linked-HashMap (LHM) using an Entry-set and Key-set. ESA and ESM denote Entry-set Access and Manipulation. KSA and KSM represent Key-set Access and Manipulation. Lines of code highlighted in green and red are LHM access and manipulation using  Entry-set and Key-set.

In this work, a code snippet or program is mediocre if it introduces a significant performance overhead to software and consequently skews the baseline resulting in an outlier performance. For example, consider the motivating example above. The snippet is a real-world fragment of a Java program, and it calculates the term frequency of more than 2000 elements in a linked hashmap (LHM). An LHM combines a hash table and a linked list. It ensures the predictable maintenance of elements in an iterable object. The peripheral difference between the snippets is in how the linked hashmap is accessed (ESA), (KSA), and manipulated (ESM), (KSM) using an ```entrySet()``` and ```keySet()``` respectively. On a test level, the ESA and ESM, and KSA and KSM versions of the code snippet execute in ```4 seconds``` and ```4 minutes``` as tested in our test environment, which means there is a ```6000% increase``` in ET. The difference decreases by half to ```3000%```, with an increase in the number of elements from 2,000 to 200,000. 

> Note: we are working under the assumption of an ```ideal``` software development environment, where external variables such as memory usage and network connectivity relatively outside the developer's control are operating at ```optimal``` levels.

## Microbenchmarking
Software performance microbenchmarking is a standardized procedure to experimentally analyze the execution time of non-functional components of the software, such as code snippets.  Test case results serve as the microbenchmarks, ground truth target variables fed to implemented predictive models for performance predictions.

## Continuous Predicitons & Deduplication
We propose continuous or rolling predictions in line with our goal of predicting the performance of the local software repository before being pushed to its remote counterpart. Encapsulated in the Figure displays base repository copy ```n``` is the repository's current code state (CCS), while ```n-1``` is a local version consisting of an updated CCS in the queue to be pushed to remote. We feed  ```n``` (training set) to a regression function ($\phi$) to predict the performance of ```n-1``` (testing set). We repeat this process for ```n-n``` times. Furthermore, it eliminates duplicated observations. 


<p align="center"> <img src="..doc/roll.svg" width="60%"> </p>

<!-- ## Code Stylometry Feature Engineering
Code-stylometry (CStyle) is the stylometric analysis of a code's syntactic and lexical characteristics. In principle, cstyle is akin to stylometry. Contrastly, in stylometry, we analyze natural languages, while cstyle involves the analyses of programming languages. 

### Taxonomy of Code Stylometry Features (CSF)
<hr>
### Feature Selection

 ```{Statements, Controls, Expressions}``` $\in$ ```Syntactic``` $\land$ ```{Invocations, Declarations}``` $\in$ ```Lexical``` 
| Class | Types | Brief Description | 
| --------------- | --------------- | --------------- |
| Statements | IfStatement, WhileStatement, DoStatement, AssertStatement, SwitchStatement, ForStatement, ContinueStatement, ReturnStatement, ThrowStatement, SynchronizedStatement, TryStatement, BreakStatement, BlockStatement, BinaryOperation, CatchClause | Dictates the behavior of a program under explicitly defined conditions |
| Controls | ForControl, EnhancedForControl 	  | Defines the repetition of instructions dependent on the satisfaction of requirements |
| Expressions | StatementExpression, TernaryExpression, LambdaExpression	  | Independent language entities with unique definitions |
| Invocations | SuperConstructorInvocation, MethodInvocation,  SuperMethodInvocation, SuperMemberReference, ExplicitConstructorInvocation, ArraySelector, AnnotationMethod, MethodReference | Defines the invocation of a program from another program |
| Declarations | TypeDeclaration, FieldDeclaration, MethodDeclaration, ConstructorDeclaration, PackageDeclaration, ClassDeclaration, EnumDeclaration, InterfaceDeclaration, AnnotationDeclaration, ConstantDeclaration, VariableDeclaration, LocalVariableDeclaration, EnumConstantDeclaration, VariableDeclarator  | Declares the existence of an entity in memory and assigns a value to that entity | -->

## Reproducing Results in Paper
RQ1: ```What is the predictive prowess of code performance given a code update?```
<hr>

- SR Features on ABD
```
$ python3.9 experiments.py -c=5 -t=sr
```

- NR Features on ABD
```
$ python3.9 experiments.py -c=5 -t=nr
```

- SR Features on DSD
```
$ python3.9 experiments.py -c=50 -t=sr
```

- NR Features on DSD
```
$ python3.9 experiments.py -c=50 -t=nr
```
RQ1: ```Continued: Predictors' Throughput (Command 1) and Latency (Command 2)```

- PACE-MLP
```
$ python3.9 experiments.py -mtp=mtp
```
```
$ python3.9 experiments.py -mtlp=mtlp
```

- PACE-SVR
```
$ python3.9 experiments.py -svp=svp
```
```
python3.9 experiments.py -svlp=svlp
```

- PACE-RFR
```
$ python3.9 experiments.py -rfp=rfp
```
```
$ python3.9 experiments.py -rflp=rflp
```

- PACE-BR
```
$ python3.9 experiments.py -brp=brp
```
```
$ python3.9 experiments.py -brlp=brlp
```
- PACE-kNN on ABD
```
$ python3.9 experiments.py -knp0=knp0
```
```
$ python3.9 experiments.py -klp0=klp0
```

- PACE-kNN on DSD 
```
$ python3.9 experiments.py -knp1=knp1
```
```
$ python3.9 experiments.py -klp1=klp1  
```

RQ2: ```What is the performance impact of commit at cn-1 given cn code update?```
```
$ python3.9 experiments.py -pimp=pimp  
```

RQ3: ```What is the cost of selecting and representing code stylometry features?```

```Throughput (Command 1) and Latency (Command 2)``` 

Selection on ABD and DSD
```
$ python3.9 experiments.py -abst=abst
```
```
$ python3.9 experiments.py -absl=absl
```
```
$ python3.9 experiments.py -dsdt=dsdt
```
```
$ python3.9 experiments.py -dsdl=dsdl
```

SR and NR on ABD and DSD
```
$ python3.9 experiments.py -absr=absr
```
```
$ python3.9 experiments.py -absrl=absrl
```
```
$ python3.9 experiments.py -abnr=abnr
```
```
$ python3.9 experiments.py -abnrl=abnrl
```
```
$ python3.9 experiments.py -dssr=dssr
```
```
$ python3.9 experiments.py -dssrl=dssrl
```
```
$ python3.9 experiments.py -dsnr=dsnr
```
```
$ python3.9 experiments.py -dsnrsl=dsnrsl
```

RQ4: ```How does PACE perform in comparison with state-of-the-art performance prediction approach in the literature?```
  - H2
```
$ python3.9 experiments.py -h2=h2
```
  - RDF4J
```
$ python3.9 experiments.py -rd=rd
```
  - DUBBO
```
$ python3.9 experiments.py -db=db
```
  - SYSTEMDS
```
$ python3.9 experiments.py -ss=ss
```
  - COMBINED
```
$ python3.9 experiments.py -cb=cb
```