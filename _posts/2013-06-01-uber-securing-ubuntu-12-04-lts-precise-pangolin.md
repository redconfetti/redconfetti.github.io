---
layout: post
title: "&Uuml;ber-Securing Ubuntu 12.04 LTS with Mod-Security + Ruby on Rails 3.2.13"
author_email: marshallsontag@gmail.com
excerpt: "I recently setup a linode server for a new client who wanted the best security
  practices installed.\r\n\r\nMost linux security guides instruct you to disable password
  authentication and enable SSH key authentication for SSH and use iptables to allow
  or block certain ports. But I found this incredible guide that takes it several
  steps further to prevent IP spoofing, DDOS attacks and much more."
date: '2013-06-01 08:29:51 -0700'
categories:
- Hosting
tags:
- security
- mod_security
- ubuntu
- DDOS
- linux
- hosting
- ssh
comments: []
---
<p>I recently setup a linode server for a new client who wanted the best security practices installed.</p>
<p>Most linux security guides instruct you to disable password authentication and enable SSH key authentication for SSH and use iptables to allow or block certain ports. But I found this incredible guide that takes it several steps further to prevent IP spoofing, DDOS attacks and much more.<a id="more"></a><a id="more-1538"></a></p>
<header id="main-content-header">
<h1 id="page-title"><a href="http://www.thefanclub.co.za/how-to/how-secure-ubuntu-1204-lts-server-part-1-basics">How to secure an Ubuntu 12.04 LTS server - Part 1 The Basics</a></h1><br />
It also incorporates mod-security rules from the <a href="https://www.owasp.org/index.php/Main_Page">Open Web Application Security Project</a>, which I just learned about through this tutorial.</p>
<p><strong>Note:</strong> Most of it installed without issue. But the fstab change stopped by server from booting. You've been warned!</p>
<p><strong>UPDATE: </strong>After deploying my Rails app to the server, I was getting 403 Forbidden errors. Naturally, I assumed it was mod-security, and I assumed there was some kind of conflict with Rails. I remembered there was a log here:</p>
<blockquote><p>/var/log/apache2/modsec_audit.log</blockquote><br />
In the log, I first saw this:</p>
<blockquote><p>Message: Access denied with code 403 (phase 1). Match of "streq %{SESSION.IP_HASH}" against "TX:ip_hash" required. [file "/etc/modsecurity/activated_rules/modsecurity_crs_16_session_hijacking.conf"] [line "35"] [id "981059"] [msg "Warning - Sticky SessionID Data Changed - IP Address Mismatch."]</blockquote><br />
Looks like it has something to do with Rails sessions. I opened up /etc/modsecurity/activated_rules/modsecurity_crs_16_session_hijacking.conf and commented out line 35, which looked like this:</p>
<blockquote><p>#SecRule TX:IP_HASH "!@streq %{SESSION.IP_HASH}" "phase:1,id:'981059',t:none,block,setvar:tx.sticky_session_anomaly=+1,msg:'Warning - Sticky Ses$</blockquote><br />
Then I reloaded Apache and got this error:</p>
<blockquote><p>Message: Access denied with code 403 (phase 1). Match of "streq %{SESSION.UA_HASH}" against "TX:ua_hash" required. [file "/etc/modsecurity/activated_rules/modsecurity_crs_16_session_hijacking.conf"] [line "36"] [id "981060"] [msg "Warning - Sticky SessionID Data Changed - User-Agent Mismatch."]</blockquote><br />
So I commented out that line, which looked like this:</p>
<blockquote><p>#SecRule TX:UA_HASH "!@streq %{SESSION.UA_HASH}" "phase:1,id:'981060',t:none,block,setvar:tx.sticky_session_anomaly=+1,msg:'Warning - Sticky Ses$</blockquote><br />
Finally, after reloading again, everything worked perfectly.</p>
<p><strong>UPDATE 2:</strong> I spoke too soon. After submitting the form for registration of an account on my freshly-deployed app, I got another 403 error. This was the culprit:</p>
<blockquote><p>Message: Access denied with code 403 (phase 2). Pattern match "(^[\"'`\xc2\xb4\xe2\x80\x99\xe2\x80\x98;]+|[\"'`\xc2\xb4\xe2\x80\x99\xe2\x80\x98;]+$)" at ARGS:utf8. [file "/etc/modsecurity/activated_rules/modsecurity_crs_41_sql_injection_attacks.conf"] [line "64"] [id "981318"] [rev "2.2.5"] [msg "SQL Injection Attack: Common Injection Testing Detected"] [data "\xe2"] [severity "CRITICAL"] [tag "WEB_ATTACK/SQL_INJECTION"] [tag "WASCTC/WASC-19"] [tag "OWASP_TOP_10/A1"] [tag "OWASP_AppSensor/CIE1"] [tag "PCI/6.5.2"]</blockquote><br />
Checking the "/etc/modsecurity/activated_rules/modsecurity_crs_41_sql_injection_attacks.conf" file, I found this:</p>
<blockquote><p># -=[ String Termination/Statement Ending Injection Testing ]=-<br />
#<br />
# Identifies common initial SQLi probing requests where attackers insert/append<br />
# quote characters to the existing normal payload to see how the app/db responds.<br />
#<br />
SecRule REQUEST_COOKIES|REQUEST_COOKIES_NAMES|REQUEST_FILENAME|ARGS_NAMES|ARGS|XML:/* "(^[\"'`&acute;&rsquo;&lsquo;;]+|[\"'`&acute;&rsquo;&lsquo;;]+$)" "phase:2,rev:'2.2.5',capture,t:none,t:urlDecodeUni,block,msg:'SQL Injection Attack: Common Injection Testing Detected',id:'981318',logdata:'%{TX.0}',severity:'2',tag:'WEB_ATTACK/SQL_INJECTION',tag:'WASCTC/WASC-19',tag:'OWASP_TOP_10/A1',tag:'OWASP_AppSensor/CIE1',tag:'PCI/6.5.2',setvar:'tx.msg=%{rule.msg}',setvar:tx.sql_injection_score=+%{tx.critical_anomaly_score},setvar:tx.anomaly_score=+%{tx.critical_anomaly_score},setvar:tx.%{rule.id}-WEB_ATTACK/SQL_INJECTION-%{matched_var_name}=%{tx.0}"</blockquote><br />
Interesting. Something about my submission was seen as a "critical" SQL injection attack involving appended quote characters. Commented that SecRule out and this was the next error:</p>
<blockquote><p>Message: Warning. Operator GE matched 1 at TX. [file "/etc/modsecurity/activated_rules/modsecurity_crs_60_correlation.conf"] [line "29"] [id "981202"] [msg "Correlated Attack Attempt Identified: (Total Score: 9, SQLi=, XSS=) Inbound Attack ( Inbound Anomaly Score: ) + Outbound Application Error (The application is not available - Outbound Anomaly Score: 4)"] [severity "ALERT"]</blockquote><br />
Naturally, I commented that out:</p>
<blockquote><p># Correlated Attack Attempt<br />
#<br />
#SecRule &amp;TX:'/AVAILABILITY\\\/APP_NOT_AVAIL/' "@ge 1" \<br />
#"chain,phase:5,id:'981202',t:none,log,pass,skipAfter:END_CORRELATION,severity:'1',msg:'Correlated Attack Attempt Identified: (Total Score: %$<br />
#SecRule &amp;TX:'/WEB_ATTACK/' "@ge 1" "t:none"</blockquote><br />
Then I got a dozen of these errors:</p>
<blockquote><p>Message: Rule 7fd309fc0280 [id "950901"][file "/etc/modsecurity/activated_rules/modsecurity_crs_41_sql_injection_attacks.conf"][line "77"] - Execution error - PCRE limits exceeded (-8): (null).</blockquote><br />
Looked up that line and it was labeled "SQL Tautologies". Commented that out and got this:</p>
<blockquote><p>Message: Warning. Match of "eq 1" against "&amp;ARGS:CSRF_TOKEN" required. [file "/etc/modsecurity/activated_rules/modsecurity_crs_43_csrf_protection.conf"] [line "31"] [id "981143"] [msg "CSRF Attack Detected - Missing CSRF Token."]<br />
Message: Access denied with code 403 (phase 4). Pattern match "^5\\d{2}$" at RESPONSE_STATUS. [file "/etc/modsecurity/activated_rules/modsecurity_crs_50_outbound.conf"] [line "53"] [id "970901"] [rev "2.2.5"] [msg "The application is not available"] [severity "ERROR"] [tag "WASCTC/WASC-13"] [tag "OWASP_TOP_10/A6"] [tag "PCI/6.5.6"]<br />
Message: Warning. Operator GE matched 4 at TX:outbound_anomaly_score. [file "/etc/modsecurity/activated_rules/modsecurity_crs_60_correlation.conf"] [line "40"] [id "981205"] [msg "Outbound Anomaly Score Exceeded (score 4): The application is not available"]</blockquote><br />
Commented out those last 3 lines, and it worked! I expect that I'll still find more rules that conflict with the default behavior of Rails.</p>
<p>Isn't it fascinating that Ruby on Rails triggers so many of the <a href="https://www.owasp.org/index.php/Main_Page">Open Web Application Security Project</a>'s security rules?</p>
<p></header></p>
