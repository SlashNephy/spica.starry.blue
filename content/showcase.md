---
title: "Showcase"
date: 2020-01-01T00:00:00+09:00
draft: false
---

ここでは, 私のおもな製作物について紹介します。

Kotlin Library
---------

> Kotlin is available at [kotlinlang.org](https://kotlinlang.org).

- [Penicillin](https://github.com/StarryBlueSky/Penicillin)  
[Kotlin 1.4+] Twitter API wrapper written in Kotlin/Multiplatform. Works on JVM, JS (Browser, NodeJS).

```kotlin
suspend fun main() {
    PenicillinClient {
        account {
            application("ConsumerKey", "ConsumerSecret")
            token("AccessToken", "AccessToken Secret")
        }
    }.use { client ->
        client.timeline.userTimelineByScreenName(
            screenName = "twitter",
            count = 100
        ).execute().forEach { status ->
            println(status.text)
        }
    }
}
```

- [Json.kt](https://github.com/StarryBlueSky/Json.kt)  
[Kotlin 1.4+] Json bindings for Kotlin/Multiplatform. This project is used in Penicillin.

```kotlin
data class Model(override val json: JsonObject): JsonModel {
    val a by int
    val b by float
    val c by string
    val d by intList
    val e by model { E(it) }
    val f by modelList { E(it) }

    data class E(override val json: JsonObject): JsonModel {
        val x by nullableString
        val y by double
        val z by int
    }
}

private val json = """{
    "a": 1,
    "b": 2.3,
    "c": "hoge",
    "d": [2, 3, 5, 7],
    "e": {
        "x": "1",
        "y": 2.0,
        "z": 3
    },
    "f": [
        {
            "x": "1",
            "y": 2.0001,
            "z": 3
        },
        {
            "x": null,
            "y": 20.00001,
            "z": 30
        }
    ]
}"""

fun main() {
    val model = json.parseObject { Model(it) }

    println(model.a == 1) // true
}
```

- [VRChaKt](https://github.com/StarryBlueSky/VRChaKt)  
[Kotlin 1.4+] VRChat Web API wrapper written in Kotlin. Currently working in progress.

Kotlin Application
---------

- [Tweetstorm](https://github.com/SlashNephy/Tweetstorm)  
A simple substitute implementation for the Twitter UserStream.

Python Library
---------

- [PyChroner](https://github.com/StarryBlueSky/PyChroner)  
[Python 3.6+] Easy bot Framework supporting Twitter & Discord.

- [Twispy](https://github.com/StarryBlueSky/Twispy)  
A Lightweight & Full Powered Twitter API Wrapper.
