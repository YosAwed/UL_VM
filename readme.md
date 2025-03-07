#ユーザー定義オブジェクトに基づく論理型言語PALの仮想マシン

##PALとは？
自然言語の理解過程を実現する枠組として、Prologが提唱されたが、高速な知識の逐次更新を行なうためにはPrologの基本オブジェクト（コンスやアトム）だけでは、表現力が不十分である。
Prologにおいて、人間は、与えられた基本オブジェクトだけを用いてプログラミングしなければならない。
このことは、人間がイメージする対象を基本オブジェクトに当てはめて表現し、その動作を
プログラムに陽に記述する必要を強いる。
そこでPALは、ユーザー定義オブジェクトとして、集合束縛変数、制約つき変数を導入することで、これらの問題を解決している。

##PAL仮想マシン命令表

PAL仮想マシンで用いられている命令セットを以下に示す。
```
                   HEAD               BODY

PROCEDUAL         proceed             exec P
                                      call P,num
                                      Call num
                  alloc               dealloc

GET/PUT           getVar R,V          putVar R,V
                  getVal R,V          putVal R,V
                  getTVal R,R         moveReg R,R
                                      putUVal R,V
                                      putTVar R
                                      putTTVar R,V
                  getSymCon R,A       putSymCon R,A
                  getNumCon R,N	      putNumCon R,N
                  getNil R            putNil R
                  getCons R           putCons R

INFORMATION                           putI R,num
                                      putIVar R,V,R
                                      putIVal R,V
                  ShiftReg num
                  getRreg V           putRreg V
                  getTRreg R          putTRreg R

CUT               getBreg V           putBreg V
                  getTBreg R          putTBreg R

CONS              bldNil
                  bldVar V
                  bldTVar R
                  bldSymCon A
                  bldNumCon N
                  bldVal V
                  bldTVal R
                  
UNIFY             uniVar V
                  uniTVar R
                  uniVal V
                  uniTVal R
                  uniSymCon A
                  uniNumCon N

INDEXING          TryMeElse L,num
                  RetryMeElse L
                  TrustMeElseFail

                  switchOnVar R,L1,L2
```
                  
##コンパイルコードの例

```
;sample 4 cut operation test
; last call optimization
; Pal source program
(as (p *x *y)(q *z *w)(r *x *y))

(as (q *x *q)(s *y *z)(cut)(t *x)
(as (q *x *y)(t *x))

(as (s a b))
(as (t k))
(as (r u v))
; Compile-code for WAE
(label (p 2))
 (alloc)
 (getVar 1 2)
 (getVar 2 3)
 (putVar 1 4)
 (putVar 2 5)
 (call (q 2) 6)
 (putVal 1 2)
 (putVal 2 3)
 (dealloc)
 (exec (r 2))
(label (q 2))
 (getTBreg 3)
 (tryMeElse 3 (q 2 2))
 (alloc)
 (getVar 3 5)   ;for Breg
 (getVar 1 2)
 (putVar 1 3)
 (putVar 2 4)
 (call (s 2) 6)
 (putBreg 5)
 (putVal 1 2)
 (dealloc)
 (exec (t 1))
(label (q 2 2))
 (trustMeElseFail 3)
 (exec (t 1))
(label (s 2))
 (getSymCon 1 a)
 (getSymCon 2 b)
 (proceed)
(label (t 1))
 (getSymCon 1 k)
 (proceed)
(label (r 2))
 (getSymCon 1 u)
 (getSymCon 2 v)
 (proceed)
```
