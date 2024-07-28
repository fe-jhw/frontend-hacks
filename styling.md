## 부모 정중앙에 자식 요소 배치하기

```css
top: 50%;
left: 50%;
transform: translate(-50%, -50%);
```

## grid 예시

```css
.innerHeader {
    width: 102rem;
    margin-left: auto;
    margin-right: auto;
    display: grid;
    grid-template-columns: auto 24.6rem 1fr auto auto;
    grid-template-rows: 7.4rem 4.2rem;
    grid-template-areas:
        "buttonCategory brand searchForm myCoupang cart"
        "buttonCategory typeNavigation typeNavigation myCoupang cart";

    .brand {
        grid-area: brand;
        margin-top: 0;
        margin-bottom: 0;
        align-self: end;
        padding-left: 4rem;
        padding-right: 3rem;
    }

    .buttonCategory {
        grid-area: buttonCategory;
        border: 0;
        padding: 4.2rem 0 0 0;
        font-size: 1.2rem;
        line-height: 1.2;
        color: var(--color-white);
        width: 11rem;
        height: 11.6rem;
        background: var(--color-primary) url('../../assets/hamburger.svg') no-repeat 50% 3rem;
    }

    .searchForm {
        grid-area: searchForm;
        align-self: end;

        fieldset {
            margin: 0;
            border: 0;
            padding: 0;
        }

        .searchFormWrapper {
            height: 4rem;
            border: 2px solid var(--color-primary);
            display: flex;
            flex-flow: row nowrap;
            align-items: center;

            .formSelect {
                position: relative;
                height: 100%;
                border-right: 1px solid var(--light-gray);

                select {
                    appearance: none;
                    border: 0;
                    width: 13.5rem;
                    height: 100%;
                    font-size: 1.2rem;
                    line-height: 1.2;
                    color: var(--dark-gray);
                    padding-left: 1.2rem;
                }

                .iconDown {
                    position: absolute;
                    right: 1rem;
                    top: 50%;
                    transform: translateY(-50%);
                    pointer-events: none;
                }
            }

            .formInput {
                flex-grow: 1;
                height: 100%;

                input {
                    border: 0;
                    width: 100%;
                    height: 100%;
                    padding: 1rem 1rem 1.3rem;
                }

                input::placeholder {
                    font-size: 1.2rem;
                    line-height: 1.2;
                    color: var(--dark-gray);
                }
            }

            .searchButton,
            .voiceSearchButton {
                border: 0;
                padding: 0;
                width: 3rem;
                height: 3rem;
                margin-right: 1rem;
            }

            .searchButton {
                order: 1;
                background: url('../../assets/search.svg') no-repeat 50% 50%;
            }

            .voiceSearchButton {
                background: url('../../assets/mic.svg') no-repeat 50% 50%;
            }

        }
    }

    .button {
        border: 0;
        cursor: pointer;
        user-select: none;
        color: inherit;
        background-color: transparent;
    }

    .myCoupang {
        position: relative;
        grid-area: myCoupang;
        margin-left: .6rem;
        font-size: 1.2rem;
        line-height: 1.2;
        align-self: center;

        .myCoupangButton {
            width: 5rem;
            height: 6rem;
            background: transparent url('../../assets/my-coupang.svg') no-repeat 50% .5rem;
            padding: 3rem 0 0 0;
        }

        .myCoupangList {
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            list-style: none;
            padding-left: 0;
            margin: 0;
            background-color: var(--color-white);
            display: none;
            flex-flow: column nowrap;
            border: 1px solid var(--light-gray);
            padding: 2rem 1.6rem;


            li {
                margin: .2rem 0;

                a {
                    white-space: nowrap;
                    display: block;
                    padding: .4rem;
                }
            }

        }

        .myCoupangList::before,
        .myCoupangList::after {
            content: '';
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
        }


        .myCoupangList::before {
            border-left: 5px solid transparent;
            border-right: 5px solid transparent;
            border-bottom: 8px solid var(--light-gray);
            top: -0.9rem;
        }

        .myCoupangList::after {
            border-left: 4px solid transparent;
            border-right: 4px solid transparent;
            border-bottom: 8px solid var(--color-white);
            top: -.8rem;
        }
    }


    .cart {
        grid-area: cart;
        align-self: center;
        font-size: 1.2rem;
        line-height: 1.2;
        margin-left: 0.6rem;
        position: relative;

        .cartButtonWrapper {
            position: relative;

            .cartButton {
                width: 5rem;
                height: 6rem;
                padding: 3rem 0 0 0;
                background: url('../../assets/cart.svg') no-repeat 50% .5rem;
            }

            .cartProductCount {
                position: absolute;
                top: 0;
                right: 0;
                background: var(--color-primary);
                color: var(--color-white);
                width: 1.8rem;
                height: 1.8rem;
                border-radius: 50%;
                text-align: center;
            }
        }
    }

    .typeNavigation {
        grid-area: typeNavigation;
        align-self: center;
    }

    .typeNavigationList {
        list-style: none;
        padding-left: 0;
        margin: 0 0 0 3.6rem;
        font-size: 0;

        li {
            display: inline-block;
            margin-left: 0.2rem;

            .badgeRocket {
                margin-left: 0.4rem;
            }

            .badgeNew {
                margin-left: 0.4rem;
            }

            a {
                font-size: 1.2rem;
                line-height: 1.2;
                display: inline-block;
                padding: .6rem 1.4rem;
            }
        }

        li:first-child {
            margin-left: 0;
        }
    }
}
```

## 특정 영역으로 남은 화면 꽉 채우고, 해당 영역 스크롤 가능하게 하기
```css
.area {
    flex: auto;
    overflow: auto
}
```

```tsx
<area className="flex-auto overflow-auto">
```

## input, textarea 안에 버튼 넣는법
input, textarea 에 버튼 넣고싶은 위치쪽 방향에 border를 버튼 너비만큼 준다.
버튼은 absolute로해서 input, textarea 안에 들어갈 수 있게 위치를 지정한다.
```tsx
<div className="relative flex h-[60px] w-full justify-center xl:w-2/3">
  <Input className="border-l-[6rem] border-transparent" />
  <Button className="absolute left-0 w-[6rem] />
</div>
```
