---
title: Js Practice
tags: javaScript
---

Js 실습을 위한 html
-------------

```html

<!DOCTYPE html>

<html>

<head>
    <meta charset='utf-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <title>Page Title</title>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <script>

        var x = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
        for (i in x)
            if (i <= 5)
                alert("Good alert")
            else if (i <= 8)
                alert("Nice")
            else
                alert("Yes")

        // -----------------------------

        var x = 0

        while (x < 6) {
            console.log(x)
            x++
        }

        // -----------------------------

        console.log(x)

        var a = 5
        var b = 8
        var c = 3
        var temp

        if (a > b) {
            temp = a
            a = b
            b = temp
        }

        if (b > c) {
            temp = b
            b = c
            c = temp
        }

        if (a > b) {
            temp = a
            a = b
            b = temp
        }

        console.log(a)
        console.log(b)
        console.log(c)

        // -----------------------------

        var arr = [5, 7, 3, 4, 9, 1, 2]

        for (i in arr)
            for (j = 0; j < 6; j++)
                if (arr[j] < arr[j + 1]) {
                    temp = arr[j]
                    arr[j] = arr[j + 1]
                    arr[j + 1] = temp
                }

        // for-in (i in arr)의 i는 0 ~ 6 / 7번 반복한다

        console.log(arr)

        // -----------------------------

        var age = prompt("나이를 알려주세요")

        switch (age) {
            case "18":
                console.log("18세")
                break;
            case "19":
                console.log("19세")
                break;
            default:
                console.log("잘못입력하셨습니다")
        }

        // -----------------------------

        var user = { "name": "김", "age": "10", "addr": "서울" }

        console.log(user.addr)
        console.log(typeof user) // Object

        // -----------------------------

        var a = [
            { "sgg_cd": "1132000000", "emd_nm": "도봉동", "node_code": "1", "emd_cd": "1132010800", "node_wkt": "POINT(127.04529148142653 37.678377701297464)", "sgg_nm": "도봉구", "type": "NODE", "sw_nm": "도봉", "sw_cd": "106", "node_id": 212884 },
            { "sgg_cd": "1132000000", "emd_nm": "도봉동", "node_code": "1", "emd_cd": "1132010800", "node_wkt": "POINT(127.04565147359 37.679143735911055)", "sgg_nm": "도봉구", "type": "NODE", "sw_nm": "도봉", "sw_cd": "106", "node_id": 212883 },
            { "sgg_cd": "1132000000", "emd_nm": "도봉동", "node_code": "1", "emd_cd": "1132010800", "node_wkt": "POINT(127.04552792145518 37.6798271688126)", "sgg_nm": "도봉구", "type": "NODE", "sw_nm": "도봉", "sw_cd": "106", "node_id": 212882 },
        ]

        console.log(a[0].node_id)
        console.log(typeof a) // Object

        // -----------------------------

        var date1 = new Date()
        var date2 = new Date(2000, 10, 10)
        var date3 = new Date('2001,11,11')
        var date4 = new Date('2002,12,12 10:10:10')

        console.log(typeof date1) // Object

        var y = date4.getFullYear()
        var m = date4.getMonth() // 0~11월 / 5월이면 4인덱스

        console.log(y)
        console.log(m)

        var a = [{ 1: 2 }]
        console.log(typeof a) // Object

        var b = [1, 2]
        console.log(typeof b) // Object

        var c = 3
        console.log(typeof c) // number
        
    </script>
</head>

<body>

</body>

</html>

```
