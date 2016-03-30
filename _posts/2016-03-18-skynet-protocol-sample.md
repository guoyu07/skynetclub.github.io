---
layout: default
title:  "skynet protocol sample"
date:   2016-03-18 16:03:19
categories: skynet
tags: [skynet]
---

<link rel="stylesheet" href="{{ "/css/protocol.css" | prepend: site.baseurl }}">
<div class="protocol">
    <h2>caption</h2>
    <p>目前制定的是我在做的<a href="https://github.com/forthxu/brave" target="_blank">一款开源横版格斗游戏</a>协议，其他类型可以参考，协议由header和body组成。</p>
    <p>header除了skynet规定的两个byte表示剩余内容长度，还有协议版本号（1 byte）、协议ID（2 byte），因此body最多只能有65533 byte。</p>
    <p>其中协议ID两个byte为了方便模块化操作又做了细分，这里简单分为8位表示模块，8位表示模块内协议，所以可以有256个模块每个模块256个协议，一般够用。</p>
    <p>body内容可以采用任意形式，string、json、xml、protobuf ...，我采用的是<a href="https://developers.google.com/protocol-buffers/" target="_blank">google protocol buffer</a>。</p>
    <p>客户端目前我采用的是开socket线程和做消息队列的模式，socket线程读到消息后放入相应队列，这里总共设置有四个队列，游戏外的队列、场景队列、副本队列、优先队列。游戏外队列用在还没有登陆的情况下，供登陆场景来读取登陆、注册等消息；处于场景时则读取场景队列，如聊天模块、好友模块，公告、活动等消息；副本模块主要用来保证战斗时的消息；优先模块一般可以直接在socket线程上直接处理，如踢玩家下线。网络部分这里依然是给出了<a href="/examples.html" target="_blank">talkbox</a>相关的例子</p>
</div>
<div class="protocol">
    <h2>header</h2>
    <p>protocol header</p>
    <div class="protocol_group">
        <div class="protocol_instance">
            <table>
                <thead>
                    <tr><th>Field Name</th><th>Field Type</th><th>Example</th><th>Notes</th></tr>
                </thead>
                <tfoot>
                    <tr><td>Total Size</td><td colspan="3">2+1+2+body</td></tr>
                </tfoot>
                <tbody>
                    <tr><td>length</td><td>short</td><td></td><td>max:65536<br/>length of version,Packet ID,body</td></tr>
                    <tr><td>version</td><td>byte</td><td>1</td><td>max:256</td></tr>
                    <tr><td>Packet ID</td><td>short</td><td>0x0101</td><td>max:65536,8 bit for module,8 bit for code</td></tr>
                    <tr><td>body</td><td>structure</td><td></td><td>max:65533</td></tr>
                </tbody>
            </table>
        </div>
    </div>
    <h2>body</h2>
    <p>protocol body is a serialized structured data which use <a href="https://developers.google.com/protocol-buffers/" target="_blank">google protocol buffer</a>.</p>
    <div class="protocol_group">
        <h3>common module<em>(0x00)</em></h3>
        <div class="protocol_instance">
            <h4>announcement</h4>
            <p><i>S->C</i></p>
            <table>
                <thead>
                    <tr><th>Packet ID</th><th>Field Name</th><th>Field Type</th><th>Example</th><th>Notes</th></tr>
                </thead>
                <tfoot>
                    <tr>
                        <td>structure</td>
                        <td colspan="4">
                        <pre>
message Announcement 
{
    required int32 Id = 1;   //id
    required string Content = 2;   //content
}
                        </pre>
                        </td>
                    </tr>
                </tfoot>
                <tbody>
                    <tr><td rowspan="2">0x0001</td><td>id</td><td>int</td><td>123</td><td></td></tr>
                    <tr><td>content</td><td>string</td><td></td><td>something for publish</td></tr>
                </tbody>
            </table>
            <p></p>
        </div>
        <div class="protocol_instance">
            <h4>money</h4>
            <p><i>S->C</i></p>
            <table>
                <thead>
                    <tr><th>Packet ID</th><th>Field Name</th><th>Field Type</th><th>Example</th><th>Notes</th></tr>
                </thead>
                <tfoot>
                    <tr>
                        <td>structure</td>
                        <td colspan="4">
                        <pre>
message Money 
{
    required int32 Type = 1;   //type
    required int32 Num = 2;   //num
}
                        </pre>
                        </td>
                    </tr>
                </tfoot>
                <tbody>
                    <tr><td rowspan="2">0x0002</td><td>type</td><td>int</td><td>1</td><td>1:gold<br/>2:diamond</td></tr>
                    <tr><td>num</td><td>int</td><td>123</td><td></td></tr>
                </tbody>
            </table>
            <p></p>
        </div>
        <div class="protocol_instance">
            <h4>life</h4>
            <p><i>S->C</i></p>
            <table>
                <thead>
                    <tr><th>Packet ID</th><th>Field Name</th><th>Field Type</th><th>Example</th><th>Notes</th></tr>
                </thead>
                <tfoot>
                    <tr>
                        <td>structure</td>
                        <td colspan="4">
                        <pre>
message Life 
{
    required int32 Type = 1;   //type
    required int32 Num = 2;   //num
}
                        </pre>
                        </td>
                    </tr>
                </tfoot>
                <tbody>
                    <tr><td rowspan="2">0x0003</td><td>type</td><td>int</td><td>1</td><td>1:physical strength<br/>2:HP<br/>3:attacker<br/>4:defensive<br/>5:speed</td></tr>
                    <tr><td>num</td><td>int</td><td>123</td><td></td></tr>
                </tbody>
            </table>
            <p></p>
        </div>
        <div class="protocol_instance">
            <h4>rank</h4>
            <p><i>S->C</i></p>
            <table>
                <thead>
                    <tr><th>Packet ID</th><th>Field Name</th><th>Field Type</th><th>Example</th><th>Notes</th></tr>
                </thead>
                <tfoot>
                    <tr>
                        <td>structure</td>
                        <td colspan="4">
                        <pre>
message Rank 
{
    required int32 Type = 1;   //type
    repeated UserRank Users = 2;   //users
}
message UserRank 
{
    required int32 Uid = 1;   //type
    required String Username = 2;   //username
    required int32 Num = 3; //num
}
                        </pre>
                        </td>
                    </tr>
                </tfoot>
                <tbody>
                    <tr><td rowspan="4">0x0003</td><td>type</td><td>int</td><td>1</td><td>1:complex<br/>2:money</td></tr>
                    <tr><td>uid</td><td>int</td><td>123</td><td></td></tr>
                    <tr><td>username</td><td>string</td><td>forthxu</td><td></td></tr>
                    <tr><td>num</td><td>int</td><td>1</td><td></td></tr>
                </tbody>
            </table>
            <p></p>
        </div>
    </div>
    <div class="protocol_group">
        <h3>user module<em>(0x01)</em></h3>
        <div class="protocol_instance">
            <h4>login</h4>
            <p><i>C->S</i></p>
            <table>
                <thead>
                    <tr><th>Packet ID</th><th>Field Name</th><th>Field Type</th><th>Example</th><th>Notes</th></tr>
                </thead>
                <tfoot>
                    <tr>
                        <td>structure</td>
                        <td colspan="4">
                        <pre>
message UserLogin 
{
    required string Username = 1;   //username
    required string Password = 2;   //password
}
                        </pre>
                        </td>
                    </tr>
                </tfoot>
                <tbody>
                    <tr><td rowspan="2">0x0101</td><td>username</td><td>string</td><td>forthxu</td><td></td></tr>
                    <tr><td>password</td><td>string</td><td>123456</td><td></td></tr>
                </tbody>
            </table>
            <p></p>
        </div>
        <div class="protocol_instance">
            <h4>login result</h4>
            <p><i>S->C</i></p>
            <table>
                <thead>
                    <tr><th>Packet ID</th><th>Field Name</th><th>Field Type</th><th>Example</th><th>Notes</th></tr>
                </thead>
                <tfoot>
                    <tr>
                        <td>structure</td>
                        <td colspan="4">
                        <pre>
message UserLogin 
{
    required string Username = 1;   //username
    required int32 Result = 2;  //result
    required string Tip = 3;    //tip
}
                        </pre>
                        </td>
                    </tr>
                </tfoot>
                <tbody>
                    <tr><td rowspan="3">0x0102</td><td>username</td><td>string</td><td>forthxu</td><td></td></tr>
                    <tr><td>result</td><td>int</td><td>0</td><td>>0:uid<br/>0:success but system error<br/>-1:no exist or wrong password<br/>-2:have login<br/>-3:forbidden</td></tr>
                    <tr><td>tip</td><td>string</td><td>login success</td><td>if success return uid</td></tr>
                </tbody>
            </table>
            <p></p>
        </div>
        <div class="protocol_instance">
            <h4>register</h4>
            <p><i>C->S</i></p>
            <table>
                <thead>
                    <tr><th>Packet ID</th><th>Field Name</th><th>Field Type</th><th>Example</th><th>Notes</th></tr>
                </thead>
                <tfoot>
                    <tr>
                        <td>structure</td>
                        <td colspan="4">
                        <pre>
message UserLogin 
{
    required string Username = 1;   //username
    required string Password = 2;   //password
}
                        </pre>
                        </td>
                    </tr>
                </tfoot>
                <tbody>
                    <tr><td rowspan="2">0x0103</td><td>username</td><td>string</td><td>forthxu</td><td></td></tr>
                    <tr><td>password</td><td>string</td><td>123456</td><td></td></tr>
                </tbody>
            </table>
            <p></p>
        </div>
        <div class="protocol_instance">
            <h4>register result</h4>
            <p><i>S->C</i></p>
            <table>
                <thead>
                    <tr><th>Packet ID</th><th>Field Name</th><th>Field Type</th><th>Example</th><th>Notes</th></tr>
                </thead>
                <tfoot>
                    <tr>
                        <td>structure</td>
                        <td colspan="4">
                        <pre>
message UserLogin 
{
    required string Username = 1;   //username
    required int32 Result = 2;  //result
    required string Tip = 3;    //tip
}
                        </pre>
                        </td>
                    </tr>
                </tfoot>
                <tbody>
                    <tr><td rowspan="3">0x0104</td><td>username</td><td>string</td><td>forthxu</td><td></td></tr>
                    <tr><td>result</td><td>int</td><td>0</td><td>>0:uid<br/>0:success but system error<br/>-1:exist<br/>-2:wrong password<br/>-3:forbidden</td></tr>
                    <tr><td>tip</td><td>string</td><td>if success return uid</td><td></td></tr>
                </tbody>
            </table>
            <p></p>
        </div>
    </div>
    <div class="protocol_group">
        <h3>chat module<em>(0x02)</em></h3>
        <div class="protocol_instance">
            <h4>msg</h4>
            <p><i>C<->S</i></p>
            <table>
                <thead>
                    <tr><th>Packet ID</th><th>Field Name</th><th>Field Type</th><th>Example</th><th>Notes</th></tr>
                </thead>
                <tfoot>
                    <tr>
                        <td>structure</td>
                        <td colspan="4">
                        <pre>
message UserLogin 
{
    required int32 From = 1;   //from
    required int32 To = 2;  //to
    required string Content = 3;    //content
}
                        </pre>
                        </td>
                    </tr>
                </tfoot>
                <tbody>
                    <tr><td rowspan="3">0x0201</td><td>from</td><td>int</td><td>123</td><td>uid</td></tr>
                    <tr><td>to</td><td>int</td><td>124</td><td>0:to system<br/>-1:to all</td></tr>
                    <tr><td>content</td><td>string</td><td>Hello World!</td><td></td></tr>
                </tbody>
            </table>
            <p></p>
        </div>
    </div>
    <div class="protocol_group">
        <h3>bag module<em>(0x03)</em></h3>
    </div>
    <div class="protocol_group">
        <h3>equipment module<em>(0x04)</em></h3>
    </div>
    <div class="protocol_group">
        <h3>skill module<em>(0x05)</em></h3>
    </div>
    <div class="protocol_group">
        <h3>task module<em>(0x06)</em></h3>
    </div>
    <div class="protocol_group">
        <h3>friend module<em>(0x07)</em></h3>
    </div>
    <div class="protocol_group">
        <h3>copy module<em>(0x08)</em></h3>
    </div>
</div>