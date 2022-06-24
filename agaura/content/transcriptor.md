---
title: "Transcriptor"
date: 2022-06-24T10:23:10+02:00
draft: true
categories: [Projects]
tags: [language]
---

<html>
<script src='https://github.com/surrsurus/text-to-ipa/blob/5826d8d31a5a6d15bc72daa35212f5e481ef3758/lib/text-to-ipa.js'></script>
<script src='https://github.com/surrsurus/text-to-ipa/blob/5826d8d31a5a6d15bc72daa35212f5e481ef3758/lib/converter-form.js'></script>
<script>
    window.onload = TextToIPA.loadDict('https://github.com/surrsurus/text-to-ipa/blob/5826d8d31a5a6d15bc72daa35212f5e481ef3758/lib/ipadict.txt');
</script>
<p>Use the following text areas to convert english to it's IPA equivalent.</p>
  <p>This is NOT case sensitive and punctuation is ignored.</p>
  <br />
  <p>Note: If you are using chrome and this is being ran locally, the dictionary will not load due to the way Chrome handles XHTTP Requests. It does handle them normally on proper websites. Check out a live example <a href="http://surrsur.us/projects/ipa/english-to-ipa.html">here</a></p>
  <br />
  <!-- Create a form div -->
  <div id="js-text-to-ipa-form">
    <ul style="list-style-type: none;">
      <!-- ipa-in is the designated ID to determine where text will be input from -->
      <li><textarea id="ipa-in" placeholder="Write text here" rows="4" cols="50"></textarea></li>
      <!-- This submit button runs the conversion on the input text
      As you can see, when this buton is pressed, the conver() function is ran
      where the input, output, and error IDs are given as parameters.
      These can be set to whatever you like, as long as inID and outID are text areas, and errID is a div.
      -->
      <li class="button"><button type="button" id="js-ipa-submit" onClick="ConverterForm.convert('ipa-in', 'ipa-out', 'ipa-err')">Convert!</button></li>
      <!-- ipa-out is the designated ID to determine where the converted text will be output to -->
      <li><textarea readonly id="ipa-out" placeholder="aʊtpʊt gəʊz hɪə" rows="4" cols="50"></textarea></li>
    </ul>
    <ul style="list-style-type: none;">
      <li><noscript><p>This converter will not work unless Javascript is enabled.</p></noscript></li>
      <li>
        <!-- ipa-err is the designated ID to determine where any errors with the translation will go -->
        <div id="ipa-err">
          <p>Errors will go here if you make any. (This will be overwritten)</p>
        </div>
      </li>
    </ul>
  </div>
</html>