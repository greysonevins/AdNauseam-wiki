I've seen a couple of instances of people claiming uBlock is not as memory efficient as claimed. Examples:

- ["I just installed it, and it uses 117MB - that's not even close"](http://www.reddit.com/r/chrome/comments/2cpogs/fast_and_light_ad_blocker_for_chrome_%C2%B5block/cjhutwz)
- ["Simply tried, did not see where the province of memory" (Google translate...)](http://bbs.kafan.cn/thread-1762885-1-1.html#pid32323303)

When uBlock launches, it loads all selected filter lists, parses the content, eliminates duplicates, then instantiates the filters using efficient internal representation. This parsing of the filter lists requires a good amount of temporary memory.

So if you look at the task manager **right after** uBlock has loaded and parsed the filter lists, you will still see uBlock's memory footprint as a result of loading all the filter lists. Still, at this point all this temporary memory has been relinquished to the browser, but the browser hasn't yet collected the freed memory to make it available for reuse.

If the browser is idle enough, before one minute has elapsed<sup>[1]</sup>, the browser should be able to [garbage collect](http://en.wikipedia.org/wiki/Garbage_collection_(computer_science)) the temporary memory which was freed by uBlock after it finished loading and parsing the filter lists:

![uBlock's memory footprint](https://raw.githubusercontent.com/gorhill/uBlock/master/doc/img/mem-footprint-at-launch-time.png)

The top image shows the memory footprint of uBlock right after launch (Chrome 64-bit) (expect similar memory footprint each time the filter lists have to be reloaded). The image at the bottom shows the memory footprint of uBlock before one minute has elapsed while the browser is idle.

Note that uBlock's baseline memory footprint won't change that much afterwards. It will likely settle a few MB above the memory footprint reached after garbage collection has occurred, whenever the garbage collector is permitted to do its job.

When reloading all filters (after changing selection of filter lists, for example), I notice uBlock's baseline memory footprint edges higher each time. I entered [an issue](https://github.com/gorhill/uBlock/issues/22) to be reminded to investigate whether anything can be done for this. Currently I think this is simply caused by cumulative memory fragmentation and there might not be anything which can be done. Typically I expect users will select a set of lists and stick to that afterward, so this would make this particular issue irrelevant.

***

[1] In latest release of Chromium (40+) I have notice that the garbage collector can be rather "lazy", i.e. meaning sometimes it takes a while before freed memory is garbage collected. It's why that when surveying memory usage it is important to **force** a garbage collection cycle using the dev console of the extension itself.