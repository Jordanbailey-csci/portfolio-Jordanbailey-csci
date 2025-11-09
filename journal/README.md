**Routhing with Expectations**

As an FPS gamer, I require peak frames. Processing audio and streaming on a PC is computationally taxing. To maintain maximum frame rates while still being able to stream, I use a combination of NDI (Network Device Interface), and the audio mixer Voicemeter Banana to send all streaming information to another computer. A feature of Voicemeter uses an audio protocol for sending and receiving digital sound between machines. 

<img width="1877" height="1038" alt="vban1" src="https://github.com/user-attachments/assets/e7b56702-4883-40ed-b7a9-af4750237a77" />

When I set up this feature in Voicemeter, called VBAN, I imagined it like plugging in a physical cable: connect the output of one PC to the input of another, and the sound should flow instantly. That’s how real-world patch cables work. *Out* goes *in*. 

So when I saw my VoiceMeeter indicators lighting up on both ends, green bars dancing in sync, I felt certain it was working. The sending PC was transmitting. The receiving PC was receiving. But when I tried to capture the sound using NDI, the stream was silent. No error messages. No broken connections.

I was convinced something was wrong with NDI, or maybe the network itself. But the real culprit was subtler: a mismatch between *my mental model* of how sound should route and the *system’s actual model*.

VoiceMeeter Banana’s VBAN system doesn’t treat “audio” as one continuous flow, it treats it as discrete buses. Each bus in VoiceMeeter (A1–A3 for hardware outputs, B1–B2 for virtual outputs) represents a specific audio path. When you enable **VBAN Out** on the sending PC, you’re not sending “everything you hear”, you’re sending the signal assigned to one specific bus. Likewise, when **VBAN In** receives that stream, it arrives as a new *hardware input* channel inside VoiceMeeter on the other PC. So while the audio *was* arriving, it was never routed into the virtual outputs that NDI listens to. It’s like connecting a guitar to an amp but forgetting to turn up the channel gain.

This was the most confusing part. Everything *looked* correct:

- VBAN indicators green
- Incoming audio meters active
- Network latency low
- No dropouts or packet errors

But in NDI, silence. I kept asking, *“If the audio is coming in, why can’t I hear it?”*

<img width="1825" height="1051" alt="VBAN2" src="https://github.com/user-attachments/assets/45f5629c-f711-4bf1-84ef-e3ca26593cf0" />

The answer: VBAN treats that incoming stream as a *hardware input source*, not a virtual output. It doesn’t automatically forward that signal anywhere, you must explicitly **route it** to one of the B outputs (B1 or B2) if you want other apps to pick it up.

<img width="912" height="726" alt="VBAN3" src="https://github.com/user-attachments/assets/3b137b41-ff3a-4023-bac3-209a51af380e" />

VoiceMeeter is powerful because it abstracts audio into flexible digital buses. But that abstraction can also break the metaphor. With physical patch cables, “output to input” is self-evident. You see it. You feel it. With VBAN, the connection is invisible, a checkbox and an IP address, so it’s easy to think sound will automatically end up where you expect it.

My mistake wasn’t technical, it was *conceptual*. My ***mental model*** of how I expected it to work did not match the ***conceptual model*** of how the interaction actually worked. I assumed “network out” meant “virtual in.” But VBAN’s model is closer to an *audio mixer* than a *cable*: everything you hear must be *routed* consciously, bus by bus.

### Design takeaway:

When digital systems borrow metaphors from the physical world —> cables, mixers, and patch bays, their interfaces should reinforce those metaphors, not break them.
A clearer visual of where VBAN inputs land or how they map to virtual outputs could make the invisible connections more realistic and easier to understand. Following the manual was not enough. 

It wasn’t NDI’s fault, nor VBAN’s. It was a small mismatch of expectations between how *I thought sound should behave* and how *the software actually defines it*. Once I understood that, the silence made sense and then, finally, the sound came through.
