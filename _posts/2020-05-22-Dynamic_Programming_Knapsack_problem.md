---
title: Dynamic Programming 1.3
date: 2020-05-22 22:24:00
tags: [DP,python]
music-id: 
youtube-video-ID: 
bilibili-video-ID: BV1tC4y1H7yz

---

# Knapsack problem

> 跃入人海，各有风雨灿烂

The knapsack problem is a problem in combinatorial optimization: Given a set of items, each with a weight and a value, determine the number of each item to include in a collection so that the total weight is less than or equal to a given limit and the total value is as large as possible. It derives its name from the problem faced by someone who is constrained by a fixed-size knapsack and must fill it with the most valuable items. The problem often arises in resource allocation where the decision makers have to choose from a set of non-divisible projects or tasks under a fixed budget or time constraint, respectively.

The knapsack problem has been studied for more than a century, with early works dating as far back as 1897. The name "knapsack problem" dates back to the early works of mathematician Tobias Dantzig (1884–1956),[2] and refers to the commonplace problem of packing the most valuable or useful items without overloading the luggage.

<div class="row">

    <div class="col-md-12">
        <div class="panel panel-default">
                    <div id="compiler" class="panel-heading">
            <form class="form-inline" role="form">
                 <button type="button" class="btn btn-success" id="submitBTN" disabled="disabled"><i class="fa fa-send-o"></i> 点击运行</button>
                                <select class="form-control" id="sel1">
                    <option value="https://c.runoob.com/compile/5649">R 在线工具</option><option value="https://c.runoob.com/compile/5648">VB.NET 在线工具</option><option value="https://c.runoob.com/compile/5577">TypeScript 在线工具</option><option value="https://c.runoob.com/compile/2960">Kotlin 在线工具</option><option value="https://c.runoob.com/compile/73">Pascal 在线工具</option><option value="https://c.runoob.com/compile/66">Lua 在线工具</option><option value="https://c.runoob.com/compile/22">Node.js 在线工具</option><option value="https://c.runoob.com/compile/21">Go 在线工具</option><option value="https://c.runoob.com/compile/20">Swift 在线工具</option><option value="https://c.runoob.com/compile/19">RUST 在线工具</option><option value="https://c.runoob.com/compile/18">Bash 在线工具</option><option value="https://c.runoob.com/compile/17">Perl 在线工具</option><option value="https://c.runoob.com/compile/16">Erlang 在线工具</option><option value="https://c.runoob.com/compile/15">Scala 在线工具</option><option value="https://c.runoob.com/compile/14">C# 在线工具</option><option value="https://c.runoob.com/compile/13">Ruby 在线工具</option><option value="https://c.runoob.com/compile/12">C++ 在线工具</option><option value="https://c.runoob.com/compile/11">C 在线工具</option><option value="https://c.runoob.com/compile/10">Java 在线工具</option><option value="https://c.runoob.com/compile/9">Python3 在线工具</option><option selected="selected" value="https://c.runoob.com/compile/6">Python 在线工具</option><option value="https://c.runoob.com/compile/1">PHP 在线工具</option>                </select>
                
                 <label class="pull-right"><strong style="font-size: 16px"><a href="http://mail.qq.com/cgi-bin/qm_share?t=qm_mailme&amp;email=ssbDyoOAgfLU3crf09venNHd3w" target="_blank"><i class="fa fa-envelope" aria-hidden="true"></i> 邮件反馈</a></strong></label>
                
                 <button type="button" class="btn btn-default" id="clearCode"><i class="fa fa-eraser" aria-hidden="true"></i> 清空</button>
            </form>
            </div>
            <div class="panel-body">
            <form role="form" id="compiler-form">
              <div class="form-group">
                <div class="row">
                  <div class="col-md-7">
                    <textarea class="form-control" id="code" name="code" rows="18" style="display: none;"># -*- coding: UTF-8 -*-
print 'Hello World!'</textarea><div class="CodeMirror cm-s-default"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5px; left: 35px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;" tabindex="0"></textarea></div><div class="CodeMirror-vscrollbar" tabindex="-1" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" tabindex="-1" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 30px; margin-bottom: -15px; border-right-width: 15px; min-height: 44px; min-width: 200.234px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><pre class="prettyprint"><span>xxxxxxxxxx</span></pre></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors"><div class="CodeMirror-cursor" style="left: 4px; top: 0px; height: 18px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -30px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class="CodeMirror-line  prettyprint" role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-comment"># -*- coding: UTF-8 -*-</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -30px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class="CodeMirror-line  prettyprint" role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-builtin">print</span> <span class="cm-string">'Hello World!'</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 15px; width: 1px; border-bottom: 0px solid transparent; top: 44px;"></div><div class="CodeMirror-gutters" style="height: 59px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 29px;"></div></div></div></div>
                  </div>
                  <div class="col-md-5">
                    <textarea placeholder="运行结果……" class="form-control" id="compiler-textarea-result" rows="18">Hello World!</textarea>
                  </div>
                </div>
              </div>
             
             
            </form>
            </div>
        </div>
    </div>
    <!--
    <div class="col-md-12">
    
        <div id="about" class="panel panel-default">
            <div class="panel-heading">概述</div>
            <div class="panel-body">
            内容
            </div>
        </div>
        
        <div id="author" class="panel panel-default">
            <div class="panel-body">
              <p>该实例由 <a target="_blank" href="http://www.runoob.com/">菜鸟教程</a> 整理。</a></p>
            </div>
        </div>
    </div>
    -->
</div>