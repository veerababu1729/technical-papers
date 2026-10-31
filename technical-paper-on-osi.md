# OSI Layer Model

The OSI (Open Systems Interconnection) model is a way to understand how data moves from one computer to another. It splits the whole process into 7 layers. Each layer has one job.

## The Seven Layers

<img width="5667" height="2834" alt="image" src="https://github.com/user-attachments/assets/3cf51d65-4cab-487f-b30c-ea71a2cfff2a" />

### Layer 7: Application Layer

This is the layer you actually use. Browsing a website, sending an email, all of it starts here. Note that it is not the app itself (like Chrome). It is the protocols the app uses to talk to the network, like HTTP and SMTP.

### Layer 6: Presentation Layer

This layer is the translator. It makes sure the data is in a form the other side can understand. It does three things: formats data, encrypts it, and compresses it.

### Layer 5: Session Layer

This layer manages the conversation. It starts a session between two computers, keeps it running, and closes it when the work is done.

### Layer 4: Transport Layer

This layer makes sure data reaches properly. It breaks big data into small segments, controls the speed so the receiver is not overloaded, and fixes errors. Think of it like a courier service that checks your parcel reaches safe and in order. TCP and UDP work here.

### Layer 3: Network Layer

This layer decides the path. It uses IP addresses to find the best route from sender to receiver, even across many networks. Routers work here.

### Layer 2: Data Link Layer

This layer moves data between devices on the same local network. It uses MAC addresses (a unique ID given to your network card) to find the right device. It also detects errors. Switches work here.

### Layer 1: Physical Layer

This is the real, physical part: cables, connectors, electrical signals, Wi-Fi waves, light pulses. It carries raw bits (0s and 1s) from one place to another.

## How It Works Together

When you send data, it goes down from Layer 7 to Layer 1. Each layer adds its own header on the way. At the receiver, data goes up from Layer 1 to Layer 7, and each layer removes its header. This makes troubleshooting easy, because you can check problems layer by layer.

**Tip to remember (top to bottom):** All People Seem To Need Data Processing.

## References

- Article - https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/
- Video - https://www.youtube.com/watch?v=vv4y_uOneC0
