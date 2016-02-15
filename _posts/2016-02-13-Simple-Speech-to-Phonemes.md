---
layout: post
title: Simple Speech to Phonemes
---
![Illustration](https://cloud.githubusercontent.com/assets/3308051/13057486/9a0ae2f0-d3ea-11e5-8d70-0243c8cabb65.jpg "Illustration")
Several applications today have close integration of speech input. Personally I think its a great feature to have speech input in any application. Although you might find many API's to do the job, wouldn't it be nice if you just have one implicit part of your code do the trick without having to run through whole bunch of stuff to integrate the API and request it every time also not to forget the limit on number of requests (50/day for Google). So here is a simple code that implements HTML5 webkit to do the trick. I used the code to derive the <a href="http://en.wikipedia.org/wiki/Phoneme">phoneme</a> from the speech which is used in several applications.

HTML Code:

{% highlight javascript %}
<div class="col-lg-6">
<h1>Results:</h1>
  <div id="results">
    <span id="final_span" class="final"></span>
    <span id="interim_span" class="interim"></span>
  </div>
  <div id="renderText">
  </div>
</div> 
{% endhighlight %}

Javascript Code:

{% highlight html %}
<script type="text/javascript">
var final_transcript = '';
var recognizing = false;
if ('webkitSpeechRecognition' in window) {
  var recognition = new webkitSpeechRecognition();
  recognition.continuous = true;
  recognition.interimResults = true;
  recognition.onstart = function() {
    recognizing = true;
  };
  recognition.onerror = function(event) {
    console.log(event.error);
  };
  recognition.onend = function() 
  {
    recognizing = false;
      jQuery.get('getResult.php?text=' + 
        final_transcript, function(data) 
      {
          document.getElementById('renderText').innerHTML 
          = data;
      } )
  };
  recognition.onresult = function(event) {
    var interim_transcript = '';
    for (var i = event.resultIndex; 
        i < event.results.length; ++i) {
      if (event.results[i].isFinal) {
        final_transcript += event.results[i][0].transcript;
      } else {
        interim_transcript += event.results[i][0].transcript;
      }
    }
    final_transcript = capitalize(final_transcript);
    final_span.innerHTML = linebreak(final_transcript);
    interim_span.innerHTML = linebreak(interim_transcript);
    
  };
}

var two_line = /\n\n/g;
var one_line = /\n/g;
function linebreak(s) {
  return s.replace(two_line, '<p></p>').replace(one_line, '<br>');
}

function capitalize(s) {
  return s.replace(s.substr(0,1), function(m) { return m.toUpperCase(); });
}

function startDictation(event) {
  if (recognizing) {
    recognition.stop();
    return;
  }
  final_transcript = '';
  recognition.lang = 'en-US';
  recognition.start();
  final_span.innerHTML = '';
  interim_span.innerHTML = '';
}
</script>  
{% endhighlight %}

Further, you need a simple php file to do the phoneme generation. Here is the PHP code:

{% highlight php %}
<?php
 $input=$_REQUEST['text'];
 $words = explode(' ', $input);
 $ch = curl_init("http://api.corrasable.com/phonemes/");
 curl_setopt($ch, CURLOPT_POST, 1);
 curl_setopt($ch, CURLOPT_POSTFIELDS, "text={$input}");
 curl_setopt($ch, CURLOPT_HEADER, 0);
 curl_exec($ch);
 curl_close($ch);
 ?>
{% endhighlight %}

I have tried the app locally and it works perfectly for me. Also I have embedded the above code into bootstrap to make it look elegant, if you want to try it out on your local machine feel free to clone the repository: <a href="https://github.com/PseudoAj/AjSpeechToPhonemes.git">https://github.com/PseudoAj/AjSpeechToPhonemes.git</a>