ＰＡＬ仮想マシン命令表

ＰＡＬ仮想マシンで用いられている命令セットを以下に示す。
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
                  
