# 쿠폰 등록 
```mermaid
sequenceDiagram
    actor user
    actor cl as client
    participant ctr as controller
    participant s as service
    participant db
    
    user->>ctr : UserInfo, CouponInfo
    ctr->>s: registerCoupon(UserInfo, CouponInfo)
    s ->> s : isValidUser()
    rect rgb(255,100,100)
        opt valid
            %% opt는 조건문과 동일하다. 코드 작성할 때 if와 동일
            s->>s: isRegistableCoupon()
            rect rgb(255,0,0)
                opt registable
                    s ->> db: update coupon list
                    s-->> ctr: success
                    ctr-->> user: success
                end
                end
                
        s-->>ctr: failure
        ctr -->> user: failure
        end
    end

```

# 쿠폰 사용
```mermaid
sequenceDiagram
    actor user
    actor cl as client
    participant ctr as controller
    participant s as service
    participant db
    
    cl -->>ctr: couponValid()
    rect rgb(255,100,100)
        opt valid
            ctr -->> cl: approve
            ctr -->> s: couponInfo()
            s -->> DB: update couponList(), couponUse()
        end
    end
    rect rgb(255,0,0)
        opt Invalid
            ctr -->> s: couponInfo()
            s -->> ctr: rejcet
            ctr -->> cl: reject
        end
    end
```

# 쿠폰 발급
```mermaid
sequenceDiagram
    actor cl as client
    participant ctr as controller
    participant s as service
    participant db
    
    cl -->> ctr: requestCouponDiagram
    rect rgb(255,100,100)
        ALT requestValid
            ctr -->> s: dataConfirm()
            rect rgb(255,0,0)
                OPT requestValid
                    s -->> ctr: reject
                    ctr -->> cl: reject
                end    
                end
                
                s -->> ctr: approve
                s -->> db: update couponList()
                ctr -->> cl: approve
        end
    end

```