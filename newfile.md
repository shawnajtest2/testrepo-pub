#Just making another file over here

```clojure
(require '[muuntaja.middleware :as middleware])

(defn echo [request]
  {:status 200
   :body (:body-params request)})

; with defaults
(def app (middleware/wrap-format echo))

(app {:headers
      {"content-type" "application/edn"
       "accept" "application/json"}
      :body "{:kikka 42}"})
; {:status 200,
;  :body #object[java.io.ByteArrayInputStream]
;  :muuntaja.core/format "application/json",
;  :headers {"Content-Type" "application/json; charset=utf-8"}}
```
