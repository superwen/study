```
yum -y install gcc gcc-c++ make flex bison gperf ruby \
  openssl-devel freetype-devel fontconfig-devel libicu-devel sqlite-devel \
  libpng-devel libjpeg-devel
  
git clone git://github.com/ariya/phantomjs.git
cd phantomjs
git checkout 2.1.1
git submodule init
git submodule update

python build.py


var page = require('webpage').create();
page.open('http://example.com', function(status) {
  console.log("Status: " + status);
  if(status === "success") {
    page.render('example.png');
  }
  phantom.exit();
});


phantomjs hello.js

https://github.com/ariya/phantomjs/blob/master/examples/rasterize.js

phantomjs rasterize.js https://dmitrybaranovskiy.github.io/raphael/polar-clock.html clock.png
phantomjs rasterize.js 'http://en.wikipedia.org/w/index.php?title=Jakarta&printable=yes' jakarta.pdf


var page = require('webpage').create();
//viewportSize being the actual size of the headless browser
page.viewportSize = { width: 1024, height: 768 };
//the clipRect is the portion of the page you are taking a screenshot of
page.clipRect = { top: 0, left: 0, width: 1024, height: 768 };
//the rest of the code is the same as the previous example
page.open('http://www.epg.huan.tv/', function() {
  page.render('github.png');
  phantom.exit();
});

phantomWebFramecontent.js

http://v.baidu.com/tv/list/filter-true+order-hot+pn-1+channel-tvplay

"use strict";
var page = require('webpage').create(),
    system = require('system'),
    address, output;

if (system.args.length != 3) {
    console.log('Usage: rasterize.js URL');
    phantom.exit(1);
} else {
    address = system.args[1];
    output = system.args[2];
    page.viewportSize = { width: 1024, height: 768 };    
    page.open(address, function (status) {
        if (status !== 'success') {
            console.log('Unable to load the address!');
            phantom.exit(1);
        } else {
            window.setTimeout(function () {
                console.log(page.frameContent);
                phantom.exit();
            }, 200);
        }
    });
}


jsdom.env(
  concatText,
  ["http://code.jquery.com/jquery.js"],
  function (err, window) {
    var $ = window.$;
    $(".wrapper-piclist li").each(function() {
      console.log(" -", $(this).find('.site-piclist_pic a').first().text());
    });
  }
);

var phantomjs = require('phantomjs-prebuilt');
var concat = require('concat-stream'), jsdom = require("jsdom");
var fs = require('fs');
var program = phantomjs.exec('phantomWebFramecontent.js', 'http://list.iqiyi.com/www/1/-------------4-2-1-iqiyi--.html')
var concatText = '';
var concatObj = concat(function(text){
  concatText = text.toString();
});
//program.stdout.pipe(process.stdout);
//program.stderr.pipe(process.stderr);
//program.stdout.pipe(fs.createWriteStream('/hwdata/www/baidu-out.txt')).pipe(concatObj);
program.stdout.pipe(concatObj);
program.on('exit', code => {
  console.log(concatText);
  jsdom.env(
    concatText,
    ["http://code.jquery.com/jquery.js"],
    function (err, window) {
      var $ = window.$;
      $(".wrapper-piclist li a").each(function() {
        console.log(" -", $(this).attr('href'));
      });
    }
  );
})
```
