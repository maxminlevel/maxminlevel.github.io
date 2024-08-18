---
layout: post
title: "My bookmarklet"
bg: "/misc/yak-shaving.jpg"
summary: "Just yak shaving"
tags: ['misc']
date: 2024-08-15

---

My bookmarklet list

<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <title>MML bookmarklet</title>
    <style>
      html {
        font-size: 20px;
        background-color: #eff1f5;
        color: #4c4f69;
        display: flex;
        justify-content: center;
        font-family: Arial, Helvetica, serif;
      }

      b {
        color: #1e66f5;
      }

      a {
        text-decoration: none;
      }

      li {
        padding-bottom: 5px;
      }

      .main {
        display: flex;
        align-items: center;
        flex-direction: column;
        flex-wrap: wrap;
        width: 550px;
        /* row-gap: 10px; */
      }

      .script {
        font-size: 25px;
        width: 400px;
        padding: 15px 35px;
        border-radius: 10px;
        text-align: center;
        color: #000000;
        background-color: #ea76cb;
        border-style: solid;
        transition-duration: 0.5s;
      }

      .script:hover {
        background-color: #eff1f5;
      }
    </style>
  </head>
  <body>
    <div class="main">
      <h3 align="center">My bookmarklet</h3>
      <ul>
        <li>
          <b>Bước 1:</b> Mở thanh bookmark trên trình duyệt của bạn bằng tổ hợp
          phím <b>Ctrl + Shift + B</b>
        </li>
        <li>
          <b>Bước 2:</b> Kéo cái thanh hồng hồng xinh xinh lên thanh bookmark mà
          bạn vừa mở
        </li>
        <li><b>Bước 3:</b> Vô trang bạn muốn chạy</li>
        <li><b>Bước 4:</b> Ấn vô cái bookmark bạn vừa tạo là xong :></li>
      </ul>

      <a
        style="text-decoration: none"
        class="script"
        cmt="encode script and convert double quote to single quote"
        href="javascript:(function()%7B(function()%7Bfunction _get(key)%7Breturn localStorage.getItem(key)%3D%3D%3D'visited'%7Dfunction isEmpty(value)%7Bvar i%3D0%3Bfor(%3Bi<list.length%3Bi%2B%2B)%7Bvar file%3Dlist%5Bi%5D%3Bif(file.type%3D%3D%3D'domain'%26%26(new RegExp('%5E'%2Bfile.value.replace(%2F%5C*%2Fg%2C'.*')%2B'%24')).test(value))return true%7Dreturn false%7Dfunction provide(uris)%7Bif(uris%26%26Array.isArray(uris))list%3Duris%7Dfunction load()%7Bvar uris%3DJSON.parse(localStorage.getItem('config'))%3Bprovide(uris)%7Dfunction callback()%7BlocalStorage.setItem('config'%2CJSON.stringify(list))%7Dfunction handleResult(result)%7Bvar doc%3D%0A%7B'type'%3A'domain'%2C'value'%3Aresult%2C'tag'%3A%5B'tab'%5D%7D%3Blist.push(doc)%3Bcallback()%7Dfunction clone(dataAndEvents)%7Blist%3DdataAndEvents%3Bcallback()%7Dvar list%3D%5B%5D%3Bload()%3Bvar asserterNames%3Ddocument.querySelectorAll('.titleline')%3BasserterNames.forEach(function(c)%7Bvar link%3Dc.querySelector('a')%3Bif(link%26%26link.href)%7Bvar path%3Dlink.href%3Bif(!_get(path)%26%26!isEmpty(path))%7Bwindow.open(path%2C'_blank')%3BlocalStorage.setItem(path%2C'visited')%7Delse console.log('Link excluded or already visited%3A'%2Cpath)%7Delse console.log('No valid href found.')%7D)%7D)()%3B%7D)()%3B"
        >Hancker new with config skip tag</a
      >
      <div style="white-space: nowrap; overflow-x: auto; max-width: 100%; padding: 10px;">
    Example config
    <div style="display: inline; border: 3px solid #fff;">
        [{"type":"domain","value":"https://arxiv.org/*","tag":["science","paper"]},{"type":"domain","value":"https://theguardian.com/*","tag":["magazine"]},{"type":"domain","value":"https://*medium.com/","tag":["science","blockIP"]},{"type":"domain","value":"https://www.ycombinator.com/*","tag":["YC"]},{"type":"importUrl","value":"","tag":["failed","CORS"]},{"type":"domain","value":"","tag":["added"]},{"type":"domain","value":"*/doi/*","tag":["science","paper"]},{"type":"domain","value":"*/github.com/*","tag":["code"]},{"type":"domain","value":"*/twitter.com/*","tag":["social"]},{"type":"domain","value":"*/pubmed./*","tag":["medicine"]},{"type":"domain","value":"*/nature.com/*","tag":["paper"]}]
        </div>
    </div>

    <a
      style="text-decoration: none"
      class="script"
      cmt="encode script and convert double quote to single quote"
      href="javascript:jQuery('.question-rankings').each(function(){var n;$(this).find('input').get(4).checked=!0});jQuery('.question-answers').each(function(){var n;$(this).find('input').get(4).checked=!0});"
      >Đánh Giá Người Học Random</a
    >

    <a
      style="text-decoration: none"
      class="script"
      cmt="encode script and convert double quote to single quote"
      href="javascript:document.title=prompt("Enter page title")??document.title;"
      >Đổi tên page</a
    >

     <a
      style="text-decoration: none"
      class="script"
      cmt="encode script and convert double quote to single quote"
      href="javascript:setTimeout(()=>location.reload(), prompt('Reload the page in seconds:')*1000)"
      >Auto reload after x seconds</a
    >

    </div>
  </body>
</html>
