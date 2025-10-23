AI-Generated Payload Detection - Package

Bu paket Sigma kuralı, örnek SIEM sorguları ve test event'leri içerir.
Amaç: LLM/AI destekli kötü amaçlı kod üretimini tespit etmek için kullanılabilecek örnek materyal sunmak.

İçindekiler

ai-payload-sigma.yml  : Sigma kuralı (regex tabanlı, experimental)

ai-payload-sentinel.kql: Microsoft Sentinel (Kusto) örnek sorgusu

ai-payload-splunk.spl : Splunk SPL örnek sorgusu

ai-payload-elastic.kql: Elastic / Kibana KQL örnek sorgusu

test_events/          : Örnek event JSON'ları (benign + trigger) (Bu pakete dahil değildir)

Hangi loglara ihtiyaç var?

Windows ortamı için:

Sysmon EventID 1 (Process Create) ve CommandLine içeriğini gönderin.

PowerShell için ScriptBlockLogging (EventID 4104) açık olursa daha iyi tespit elde edilir.

Linux / Container:

Process accounting / auditd / container runtime logs ile komut satırı yakalanmalı.

Nasıl test edilir? (özet)

Test VM veya isolated ortam oluşturun.

Agent ve Sysmon yüklü, PowerShell ScriptBlockLogging aktif olsun.

Benzer komutları komut satırlarında çalıştırın.

İlgili SIEM sorgusunu kopyala-yapıştır yapın ve çalıştırın.

False positive görüldüğünde falsepositives_tuning bölümünü uygulayın (whitelist vs).

Sigmac kullanımı (çevirme)

Sigmac ile farklı SIEM formatlarına çevirebilirsiniz:

pip install sigmatools
sigmac -t splunk ai-payload-sigma.yml > ai-payload-splunk.spl
sigmac -t sentinel ai-payload-sigma.yml > ai-payload-sentinel.kql
sigmac -t es-qs ai-payload-sigma.yml > ai-payload-elastic.kql


Uyarılar

status: experimental - üretimde kullanmadan önce mutlaka test edin.

Regex'ler bazı SIEM hedeflerinde küçük uyumsuzluk gösterebilir; gerektiğinde hedefe göre düzeltin.

Geliştirici makineleri / CI / notebook'lar yüksek false positive üretebilir; whitelist önerilir.

Quick tuning önerileri

Whitelist: Image, ParentImage, Computer, User.

Ek sinyaller: Outbound network, DNS anormallikleri, yeni servisler/scheduled tasks.

Rate limiting: Çok kısa sürede aynı host'tan çok sayıda tetik gelirse toplu inceleme.