# FAQ

#### Why is flatpak included? Should I use flatpak?

https://github.com/secureblue/secureblue/issues/125#issuecomment-1859610560

#### Should I use electron apps? Why don't they work well with hardened_malloc?

https://github.com/secureblue/secureblue/issues/193#issuecomment-1953323680

#### My fans are really loud, is this normal?

During rpm-ostree operations, it's normal. Outside of that:

- Make sure you followed the nvidia steps in the readme if you're using nvidia.
- Make sure you're using an `asus` image if you're using asus.

#### Should I use firejail?

[No](https://madaidans-insecurities.github.io/linux.html#firejail), use ``bubblejail`` if there's no flatpak available for an app. 

#### An app I use won't start due to a malloc issue. How do I fix it?

Override `LD_PRELOAD` for that app. For flatpaks, this is as simple as removing the environment variable via Flatseal.

#### On secureblue half of my CPU cores are gone. Why is this?

`mitigations=auto,nosmt` is set on secureblue. This means that if your CPU is vulnerable to attacks that utilize [Simultaneous Multithreading](https://en.wikipedia.org/wiki/Simultaneous_multithreading), SMT will be disabled.

#### Should I use a userns image or not? What's the difference?

[USERNS](USERNS.md)

#### How do I install `x`?

1. Check if it's already installed using `rpm -qa | grep x`
2. Check if there's a flatpak available at https://flathub.org
3. Consider using distrobox or nix to install it
4. Layer it using `rpm-ostree install`, as a last option

#### Another security project has a feature that's missing in secureblue, can you add it?

First check if the README already has an equivalent or better feature. If it doesn't, open a new github issue.

#### I need Javascript JIT for a specific site, how do I enable it?

Add an additional chromium policy file and set the sites that need JIT in `JavaScriptJitAllowedForSites`. Here is the [policy reference](
https://admx.help/?Category=Chrome&Policy=Google.Policies.Chrome::JavaScriptJitAllowedForSites).

#### How do I install steam?

To use steam you can either:

- Install the [flatpak](https://flathub.org/apps/com.valvesoftware.Steam)
- Layer the rpm with `rpm-ostree install steam`

#### Why are bluetooth kernel modules disabled? How do I enable them?

Bluetooth has a long and consistent history of security issues. However, if you still need it, run `ujust toggle-bluetooth-modules`

#### Why are upgrades so large?

This is an issue with rpm-ostree image-based systems generally, and not specific to secureblue. Ideally upgrades would come in the form of a zstd-compressed container diff, but it's not there yet. Check out [this upstream issue](https://github.com/coreos/rpm-ostree/issues/4012) for more information.

#### Why can't I install new KDE themes?

The functionality that provides this, called GHNS, is disabled by default due to the risk posed by the installation of potentially damaging or malicious scripts. This has caused [real damage](https://blog.davidedmundson.co.uk/blog/kde-store-content/). 

If you still want to enable this functionality, run `ujust toggle-ghns`

#### My clock is wrong and it's not getting automatically set. How do I fix this?

If your system time is off by an excessive amount due to rare conditions like a CMOS reset, your network will not connect. A one-time manual reset will fix this. This should never be required except under very rare circumstances.

For more technical detail, see [#268](https://github.com/secureblue/secureblue/issues/268)
