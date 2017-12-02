# okhttp restful 服务开发

## 客户端：

https://github.com/ruier/android_restful_json_client.git



Android 需要 `okhttp` 库的支持，同时需要支持库 `okio` ：

```bash
https://repo1.maven.org/maven2/com/squareup/okhttp3/okhttp-android-support/3.9.0/okhttp-android-support-3.9.0.jar
https://repo1.maven.org/maven2/com/squareup/okio/okio/1.9.0/okio-1.9.0.jar
```

java code:

```java
    public static final MediaType JSON
            = MediaType.parse("application/json; charset=utf-8");

    OkHttpClient client = new OkHttpClient();

    String post(String url, String json) throws IOException {
        RequestBody body = RequestBody.create(JSON, json);
        Request request = new Request.Builder()
                .url(url)
                .post(body)
                .build();
        try (Response response = client.newCall(request).execute()) {
            return response.body().string();
        }
    }

    String run(String url) throws IOException {
        Request request = new Request.Builder()
                .url(url)
                .build();

        try (Response response = client.newCall(request).execute()) {
            return response.body().string();
        }
    }
    
    
            new Thread()  {
            @Override
            public void run()
            {
                try {
                    String response = MainActivity.this.run("http://10.56.56.236:65500/");
                    Log.i("kemov", "respond" + response);
                }catch (Exception e) {
                }

            }
        }.start();
```







## 服务端：

 https://github.com/ruier/flask_restful_webservice.git

python + flask ：

```bash
sudo apt install python-pip
pip install flask
vi web_service.py
python web_service.py
```

demo code:

```python
#!/bin/python
from flask import Flask, jsonify

app = Flask(__name__)

tasks = [
    {
        'id': 1,
        'title': u'Buy groceries',
        'description': u'Milk, Cheese, Pizza, Fruit, Tylenol',
        'done': False
    },
    {
        'id': 2,
        'title': u'Learn Python',
        'description': u'Need to find a good Python tutorial on the web',
        'done': False
    }
]

@app.route('/todo/api/v1.0/tasks', methods=['GET'])
def get_tasks():
    return jsonify({'tasks': tasks})

if __name__ == '__main__':
    app.run(debug=True,host="10.56.56.236",port=65500)

```



https://www.cnblogs.com/vovlie/p/4178077.html

http://61.183.69.226:65500/

http://10.56.56.236:65500/

61.183.69.226 做了端口绑定到 10.56.56.236（本地局域网）65500

## 效果

```bash
130|shell@T82S:/ $ logcat -s kemov
--------- beginning of system
--------- beginning of main
^C
130|shell@T82S:/ $ logcat -s kemov
--------- beginning of system
--------- beginning of main
I/kemov   (25128): jSON{"modid":"qwe","name":"ddr","status":"wee","remark":"444"}
I/kemov   (25256): jSON{"modid":"yyy","name":"hhj","status":"寮?,"remark":"hhj"}



I/kemov   (25704): jSON{"modid":"123","name":"abc","status":"ok","remark":"good"}
I/kemov   (25704): respond<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
I/kemov   (25704): <title>404 Not Found</title>
I/kemov   (25704): <h1>Not Found</h1>
I/kemov   (25704): <p>The requested URL was not found on the server.  If you entered the URL manually please check your spelling and try again.</p>
```

