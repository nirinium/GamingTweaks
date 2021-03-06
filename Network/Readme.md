Intel Adapter 'Recommended' Network Adapter Settings
------------------------

Keep in mind that some drivers might have different settings/options! After talking with NordVPN I figured out with them that some settings might directly have a negative effect on Down-/Upload speed. Ensure you test each settings one by one. In this case try to disable all "offloading" params.



#### Optimized for quick response and low latency (Gaming):

* Disable Interrupt Moderation Rate (if not possible decrease the size)
* Disable Offload TCP Segmentation
* Increase Transmit Descriptors
* Increase Receive Descriptors
* Increase RSS Queues



#### Optimized for throughput (e.g. Servers):

* Enable Jumbo Frames
* Increase Transmit Descriptors
* Increase Receive Descriptors



#### For low CPU utilization:

* Maximize Interrupt Moderation Rate
* Keep Receive Descriptors at default
* Avoid setting large Receive Descriptors
* Decrease RSS Queues
* Decrease the Max number of RSS CPUs in Hyper-V environments



#### Enable or disable QoS?

It depends on how you set up your QoS/WMM. QoS is useful in scenarios where your network experiences peak or near-peak loads on a routine basis. If the network is not under significant load, QoS rules will not be applied, the same goes if your router is not configured to use QoS or the firmware doesn't includes it.

For example, if you live alone, don't play online games, don't use VOIP, don't use video streaming services (Netflix/Amazon/Hulu/Twitch.tv & YouTube), or don't use P2P file sharing - QoS is not going to be all that beneficial to you.

* Check if you're ISP offers a QoS package to residential connections (most do)
* Check your Router QoS settings
* Check your OS QoS options 

In general **I recommend to leave it enabled**. If there some problems check your configuration instead, I did some benchmarks and I never found any impact (because my router isn't confired to use it). It's right that Windows reserves some bandwith but that doesn't matter because your router might ignore the flags. 


### Optional network tweaks
___


* Disable "Inbound Layer 7" in your router.
Layer 7 filtering or shaping is identifying traffic at layer 7 (Application Layer) of the OSI model. Instead of shaping/filtering based on the port and source/destination, you are identifying a stream based on its contents. This is also sometimes called deep packet inspection since it works by looking into the contents of the packets not just the headers. If you're concerned about performance: IPP2P and especially L7 are slower than simple IP, MAC or port matches. L7 can't cope well with encrypted P2P traffic in background while gaming.
* Enable SYN cookies
* Disable IPv6 tunnel adapter & interfaces
* Disable all ISATAP, 6to4 and Teredo Tunneling interfaces
* Ensure Windows Defender Core Isolation stays disabled (default) it can causes 'lags' with several older games
* Disable the Timeline function, it eats HDD space in order to collect your history or if you use it, ensure e.g. CCleaner cleans it -> bigger file means longer loading times.

```bash
netsh interface teredo set state disable
netsh interface 6to4 set state disable disable
netsh interface isatap set state disable
REG ADD "HKLM\SYSTEM\CurrentControlSet\services\TCPIP6\Parameters" /v "EnableICSIPv6" /t REG_DWORD /d 0 /f
REG ADD "HKLM\SYSTEM\CurrentControlSet\services\TCPIP6\Parameters" /v "DisabledComponents" /t REG_DWORD /d 255 /f
```


Keep in mind that every modern Router firmware has an option to block or at least filter Teredo traffic, same like NetBios which means you don't need to 'disable' everything blindly because nothing can pass the router anyway!
