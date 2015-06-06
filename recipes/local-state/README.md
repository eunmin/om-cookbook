# Local State

![alt text](preview.gif "Preview Image GIF")

1) Chestnut 템플릿으로 Om 프로젝트를 만든다.

```bash
lein new chestnut local-state -- --om-tools --http-kit
```

2) 주로 사용하는 에디터로 `local-state/src/cljs/core.cljs` 파일을 연다.

3) 여기서는 `(defonce app-state ...)`는 필요 없으니 지운다.

4) init-state 함수가 있는 Om 컴포넌트를 만든다.

```clojure
(defcomponent local-state-counter-view [_ owner]
  (init-state [_])
  (render-state [_ state]))
```

5) 초기화 할 state 값을 설정한다.

```clojure
(defcomponent local-state-counter-view [_ owner]
  (init-state
   [_]
   {:button-presses 1})
  (render-state [_ state]))
```

6) 초기화 한 state를 보여주도록 render-state 함수를 만든다.

```clojure
(defcomponent local-state-counter-view [_ owner]
  (init-state
   [_]
   {:button-presses 1})
  (render-state
   [_ state]
   (dom/div
    (dom/button {} "Click Me")
    (dom/br {})
    (str "Button Presses: "
         (:button-presses state)))))

```

7) "Click Me" 버튼에 자바스크립트 onClick 이벤트를 추가한다. 클릭 이벤트에서 `om/update-state!`로 로컬 state를 바꾼다.

```clojure
(defcomponent local-state-counter-view [_ owner]
  (init-state
   [_]
   {:button-presses 1})
  (render-state
   [_ state]
   (dom/div
    (dom/button
     {:on-click (fn [_]
                  (om/update-state! owner
                                    [:button-presses]
                                    inc))}

     "Click Me")
    (dom/br {})
    (str "Button Presses: "
         (:button-presses state)))))

```

8) 로컬 state 컴포넌트가 웹페이지 표시되도록 `(defn main [] ...)` 함수를 고치거나 아래 내용으로 바꾼다.

```clojure
(defn main []
  (om/root
   local-state-counter-view
   {}
   {:target (. js/document (getElementById "app"))}))
```

9) `lein repl`로 REPL을 시작한다. 

```
nREPL server started on port 54879 on host 127.0.0.1 - nrepl://127.0.0.1:54879
REPL-y 0.3.5, nREPL 0.2.6
Clojure 1.6.0
Java HotSpot(TM) 64-Bit Server VM 1.8.0_05-b13
Docs: (doc function-name-here)
(find-doc "part-of-name-here")
Source: (source function-name-here)
Javadoc: (javadoc java-object-or-class-here)
Exit: Control+D or (exit) or (quit)
Results: Stored in vars *1, *2, *3, an exception in *e
```

10) `run` 함수를 부르면 클로저스크립트가 컴파일되고 어플리케이션을 시작된다.

```
local-state.server=> (run)
Starting figwheel.
Starting web server on port 10555 .
#<clojure.lang.AFunction$1@336fc74>
local-state.server=> Compiling ClojureScript.
Figwheel: Starting server at http://localhost:3449
Figwheel: Serving files from '(dev-resources|resources)/public'
Compiling "resources/public/js/app.js" from ("src/cljs" "env/dev/cljs")...
Successfully compiled "resources/public/js/app.js" in 18.01 seconds.
notifying browser that file changed:  /js/out/local_state/core.js
```

11) 브라우저에서 http://localhost:port에 접속한다. 다음과 같은 REPL 메시지에서 포트를 확인할 수 있다. =>  `Starting web server on port ...`
