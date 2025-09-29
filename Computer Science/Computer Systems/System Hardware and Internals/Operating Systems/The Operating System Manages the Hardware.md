#computer-system #programming 

Back to our [[Hello World Program]] example. ==When the shell loaded and ran the hello program, and when the hello program printed its message, neither the program accessed the keyboard, display, or main memory directly.==

![](Figure1.10.png)
![](Figure1.11.png)

Rather, ==they relied on the services provided by the **[[Operating system]]**==. We can think 
We can ==think of the operating system as a layer of software interposed between the application program and the hardware== as **shown in Figure 1.10.**

==All attempts by an application program to manipulate the hardware must go through the operating system==

**The operating system has two primary purposes:** 

* **(1)** ==To protect the hardware from misuse by runaway applications==

* **(2)** ==To provide applications with simple and uniform mechanisms for manipulating  complicated and often wildly different low-level hardware devices==

The ==operating system achieves both goals via the fundamental abstractions== **shown in Figure 1.11**

==**[[Processes]], [[Virtual memory]], and [[Files]]**.== As this figure suggests, **==Files** are abstractions for **[[IO Devices]]**,== ==**Virtual Memory** is an abstraction for both the **[[Main Memory]] and disk [[IO Device]]==,** and **Processes** are abstractions for the **[[Central Processing Unit (CPU)]], [[Main Memory]], [[Files]].**

---

### Processes

When a program such a [[Hello World Program]] runs on a modern system, **==The operating system provides the illusion that the program is the only one running on the system.==**  

==The program appears to have exclusive use of both the Processor, Main memory and IO Devices==

==The processor appears to execute the instruction in the program, one after other, without interruptions==. And the ==code and data of the program appears to be the only objects in the system's memory==.

==These illusions are provided by the notion of a process==, one of the most important and successful ideas in computer science.

==A **Process** is the operating system's abstraction for running program==.

==Multiple processes can run concurrently on the same system==, and ==each process== appears to ==have exclusive use of the hardware==.

By ==concurrently we mean that the instruction of one process are interleaved with the instructions of another process==. In most systems, ==there are more processes to run than there are CPU(s) to run them==.

Traditional systems could only execute  one program at a time, ==while newer **multi-core processors** can execute several programs simultaneously.== 

In either case, ==a single CPU can appear to execute multiple processes concurrently by having the processor switch among them.==

The ==operating system performs this interleaving== with a mechanism ==known as **context switching**.==

 To simplify the rest of this discussion, we consider only a uni processor system containing a single CPU. We will return to the discussion of multiprocessor systems in Section 1.9.2.
 
**==The operating system keeps track of all the state information that the process needs in order to run**.== This state, ==which is known as the context, includes information such as the current values of the PC, the register file, and the contents of main memory.== 

At any point in time, ==a uni processor system can only execute the code for a single process.== **When the operating system decides to transfer control from the current process to some new process,** ==it performs a context switch by saving the context of the current process, restoring the context of the new process, and then passing control to the new process. The new process picks up exactly where it left off.==

---

**Figure 1.12** shows the basic idea for our example hello scenario.

![](Figure1.12.png)


**There are two concurrent processes in our example scenario:** 

==The shell process and the hello process. Initially, the shell process is running alone, waiting for input on the command line.== When we ask it to run the hello program, ==the shell carries out our request by invoking a special function known as a system call that passes control to the operating system.== 

==The operating system saves the shell’s context, creates a new hello process and its context, and then passes control to the new hello process.== 

==After hello terminates, the operating system restores the context of the shell process and passes control back to it, where it waits for the next command-line input.==

As Figure 1.12 indicates, the transition from one process to another is managed by the ***operating system kernel.*** 

---

==The **[[Kernel]]** is the portion of the operating system code== that is always resident in memory. ==When an application program requires some action by the operating system, such as to read or write a file, it executes a special system call instruction, transferring control to the kernel.==

The kernel then performs the requested operation and returns back to the application program.

---
> **NOTE** 
```
That the kernel is not a separate process. Instead, it is a collection of code
and data structures that the system uses to manage all the processes.
```
---

**==Implementing the process abstraction requires close cooperation between both the low-level hardware and the operating system software.==** We will explore how this works, and how applications can create and control their own processes,
in Chapter 8.

---
### Threads

**Although we normally think of a process as having a single control flow**, ==in modern systems a process can actually consist of multiple execution units,==
==called ***[[Threads]]***, each running in the context of the process and sharing the same code and global data.==

==Threads are an increasingly important programming model because of the requirement for concurrency in network servers, because it is easier to share data between multiple threads than between multiple processes, and because threads are typically more efficient than processes.== 

==***[[Multi-threading]]*** is also one way to make programs run faster when multiple processors are available.==

![](Figure1.13.png)

> **Note** 

Refer to **[[Concurrency and Parallelism]]** for further explanation.

---

### Virtual Memory 


***[[Virtual memory]]*** is an ==abstraction that provides each process with the illusion that it has exclusive use of the main memory==. 

==Each process has the same uniform view of memory==, which is known as its **virtual address space.** The virtual address space for Linux processes is **shown in Figure 1.13.**(Other Unix systems use a similar layout.)

In Linux, the topmost region of the address space is reserved for code and data in the operating system that is common to all processes

The lower region of the address space holds the code and data defined by the user’s process Note that addresses in the figure increase from the bottom to the top.

The virtual address space seen by each process consists of a number of well-defined areas, each with a specific purpose.

* ==**Program code and data.**== Code begins at the same fixed address for all processes,followed by data locations that correspond to global C variables. The code and data areas are initialized directly from the contents of an executable object file—in our case, the hello executable. You will learn more about this part of the address space when we study linking and loading in Chapter 7.

* ==**Heap.**== The code and data areas are followed immediately by the run-time heap.
  Unlike the code and data areas, which are fixed in size once the process begins running, the heap expands and contracts dynamically at run time as a result
  of calls to C standard library routines such as malloc and free. We will study
  heaps in detail when we learn about managing virtual memory in Chapter 9.

* ==**Shared libraries.**== Near the middle of the address space is an area that holds the
  code and data for shared libraries such as the C standard library and the math
  library. The notion of a shared library is a powerful but somewhat difficult
  concept. You will learn how they work when we study dynamic linking in
  Chapter 7.

* ==**Stack**.== At the top of the user’s virtual address space is the user stack that
  the compiler uses to implement function calls. Like the heap, the user stack
  expands and contracts dynamically during the execution of the program. In
  particular, each time we call a function, the stack grows. Each time we return
  from a function, it contracts. You will learn how the compiler uses the stack
  in Chapter 3.

* ==**Kernel virtual memory**.== The top region of the address space is reserved for the
  kernel. Application programs are not allowed to read or write the contents of
  this area or to directly call functions defined in the kernel code. Instead, they
  must invoke the kernel to perform these operations.

==For virtual memory to work, a sophisticated interaction is required between==
==the hardware and the operating system software,== including a hardware translation
of every address generated by the processor. The basic idea is to store the contents
of a process’s virtual memory on disk and then use the main memory as a cache
for the disk. Chapter 9 explains how this works and why it is so important to the
operation of modern systems

---

### Files

==A file is a sequence of bytes, nothing more and nothing less. Every I/O device,==
==including disks, keyboards, displays, and even networks, is modeled as a file.== 

==All input and output in the system is performed by reading and writing files==, ==using== a
==small set of system calls known as Unix I/O.==

This simple and elegant notion of a file is nonetheless very powerful because it provides applications with a ==uniform view of all the varied I/O devices that might be contained in the system.== 

For example, ==application programmers who manipulate the contents of a disk file are blissfully unaware of the specific disk technology==. Further, ==the same program will run on different systems that use different disk technologies==. You will learn about Unix I/O in Chapter 10.

---

### Systems Communicate with Other Systems Using Networks

Up to this point in our tour of systems, we have treated a system as an isolated
collection of hardware and software. In practice, ==modern systems are often linked==
==to other systems by networks.== 

==From the point of view of an individual system, network can be viewed as just another I/O device,== as **shown in Figure 1.14**. 

When ==the system copies a sequence of bytes from main memory to the network
adapter==, ==the data flow across the network to another machine==, instead of, say, to a local disk drive. 

Similarly, ==the system can read data sent from other machines and copy these data to its main memory.==

![](Figure1.14.png)


With the advent of global networks such as the Internet, ==copying information from one machine to another has become one of the most important uses of computer systems.==

For example, applications such as email, ==instant messaging, the World Wide Web, FTP, and telnet are all based on the ability to copy information over a network.==

Returning to our hello example, ==we could use the familiar telnet application==
==to run hello on a remote machine.==

**Suppose we use a telnet client running on our local machine to connect to a telnet server on a remote machine.**

 > **Over the Telnet Example**

![](Figure1.15.png)

After we log in to the remote machine and run a shell, the remote shell is waiting to receive an input command.  
From this point, running the `hello` program remotely involves the **five basic steps** shown in *Figure 1.15*:

* We type in the `hello` string to the **telnet client** and hit the Enter key.
* The client sends the string to the **telnet server**.
* The telnet server receives the string and passes it to the **remote shell program**.
* The remote shell runs the `hello` program and passes the **output** to the telnet server.
* The telnet server forwards the output string across the network to the telnet client, which prints it on our **local terminal**.

This type of exchange between clients and servers is typical of all **network applications.**

In Chapter 11 you will ==**Learn how to build network applications and apply this knowledge to build a simple Web server.**==


---

### Conclusion

This concludes our initial whirlwind tour of systems. ==An important idea to take away from this discussion is that a system is more than just hardware.== 

==It is a collection of intertwined hardware and systems software that must cooperate in order to achieve the ultimate goal of running application programs.== 

The rest of this notes will fill in some details about the hardware and the software, and it will show how, by knowing these details, you can write programs that are faster, more reliable, and more secure.


---

#### Concurrency and Parallelism

==Throughout the history of digital computers, two demands have been constant forces in driving improvements:== 

* ==we want them to do more==
* ==we want them to run faster.== 

Both of these factors improve when the processor does more things at once. We use the term concurrency to refer to the general concept of a system with multiple, simultaneous activities, 

And the term parallelism to refer to the use of concurrency to make a system run faster. 

Parallelism can be exploited at multiple levels of abstraction in a computer system. We highlight three levels here, working from the highest to the lowest level in the system hierarchy.

**For more information**: [[Concurrency and Parallelism]]

---

#### The Importance of Abstractions in Computer Systems

The use of abstractions is one of the most important concepts in computer science. For example, one aspect of good programming practice is to formulate a simple application program interface (API) for a set of functions that allow programmers to use the code without having to delve into its inner workings.

![](virtual-memory-abstraction.png)

==Different Programming languages provide different forms and levels of support for abstraction, such as Java class declarations and C function prototypes.== 

We have already been introduced to several of the abstractions seen in computer systems, as indicated in `Figure 1.18.` ==On the processor side, the instruction set architecture provides an abstraction of the actual processor hardware. With this abstraction, a machine-code program behaves as if it were executed on a processor that performs just one instruction at a time.== 

==The underlying hardware is far more elaborate, executing multiple instructions in parallel, but always in a way that is consistent with the simple, sequential model.== 

**By keeping the same execution model, different processor implementations can execute the same machine code while offering a range of cost and performance.** 

==On the operating system side, we have introduced three abstractions: files as an abstraction of I/O devices, virtual memory as an abstraction of program memory, and processes as an abstraction of a running program.== 

To these abstractions we add a new one: **the virtual machine, providing an abstraction of the entire computer, including the operating system, the processor, and the programs.** 

The idea of a virtual machine was introduced by IBM in the 1960s, but it has become more prominent recently as a way to manage computers that must be able to run programs designed for multiple operating systems (such as Microsoft Windows, Mac OS X, and Linux) or different versions of the same operating system.

---


> [!Summary of the important of abstraction]

- **Abstraction** hides complexity. You use a feature without worrying about how it works inside.
- **Programming example:** An API lets you call functions without reading all the code behind them.
- **Languages help:**

    - Java → classes        
    - C → function prototypes

> **Abstractions in Computer Systems**

**Processor side:**

- Instruction Set Architecture (ISA) makes hardware look simple.
- A program acts like it runs one instruction at a time.
- Reality: hardware executes many instructions in parallel but still keeps results consistent.#define GLSLIFY 1

vec2 mapRefract(vec3 p);
vec2 mapSolid(vec3 p);

vec2 calcRayIntersection_3975550108(vec3 rayOrigin, vec3 rayDir, float maxd, float precis) {
  float latest = precis * 2.0;
  float dist   = +0.0;
  float type   = -1.0;
  vec2  res    = vec2(-1.0, -1.0);

  for (int i = 0; i < 50; i++) {
    if (latest < precis || dist > maxd) break;

    vec2 result = mapRefract(rayOrigin + rayDir * dist);

    latest = result.x;
    type   = result.y;
    dist  += latest;
  }

  if (dist < maxd) {
    res = vec2(dist, type);
  }

  return res;
}

vec2 calcRayIntersection_3975550108(vec3 rayOrigin, vec3 rayDir) {
  return calcRayIntersection_3975550108(rayOrigin, rayDir, 20.0, 0.001);
}

vec2 calcRayIntersection_766934105(vec3 rayOrigin, vec3 rayDir, float maxd, float precis) {
  float latest = precis * 2.0;
  float dist   = +0.0;
  float type   = -1.0;
  vec2  res    = vec2(-1.0, -1.0);

  for (int i = 0; i < 60; i++) {
    if (latest < precis || dist > maxd) break;

    vec2 result = mapSolid(rayOrigin + rayDir * dist);

    latest = result.x;
    type   = result.y;
    dist  += latest;
  }

  if (dist < maxd) {
    res = vec2(dist, type);
  }

  return res;
}

vec2 calcRayIntersection_766934105(vec3 rayOrigin, vec3 rayDir) {
  return calcRayIntersection_766934105(rayOrigin, rayDir, 20.0, 0.001);
}

vec3 calcNormal_3606979787(vec3 pos, float eps) {
  const vec3 v1 = vec3( 1.0,-1.0,-1.0);
  const vec3 v2 = vec3(-1.0,-1.0, 1.0);
  const vec3 v3 = vec3(-1.0, 1.0,-1.0);
  const vec3 v4 = vec3( 1.0, 1.0, 1.0);

  return normalize( v1 * mapRefract( pos + v1*eps ).x +
                    v2 * mapRefract( pos + v2*eps ).x +
                    v3 * mapRefract( pos + v3*eps ).x +
                    v4 * mapRefract( pos + v4*eps ).x );
}

vec3 calcNormal_3606979787(vec3 pos) {
  return calcNormal_3606979787(pos, 0.002);
}

vec3 calcNormal_1245821463(vec3 pos, float eps) {
  const vec3 v1 = vec3( 1.0,-1.0,-1.0);
  const vec3 v2 = vec3(-1.0,-1.0, 1.0);
  const vec3 v3 = vec3(-1.0, 1.0,-1.0);
  const vec3 v4 = vec3( 1.0, 1.0, 1.0);

  return normalize( v1 * mapSolid( pos + v1*eps ).x +
                    v2 * mapSolid( pos + v2*eps ).x +
                    v3 * mapSolid( pos + v3*eps ).x +
                    v4 * mapSolid( pos + v4*eps ).x );
}

vec3 calcNormal_1245821463(vec3 pos) {
  return calcNormal_1245821463(pos, 0.002);
}

float beckmannDistribution_2315452051(float x, float roughness) {
  float NdotH = max(x, 0.0001);
  float cos2Alpha = NdotH * NdotH;
  float tan2Alpha = (cos2Alpha - 1.0) / cos2Alpha;
  float roughness2 = roughness * roughness;
  float denom = 3.141592653589793 * roughness2 * cos2Alpha * cos2Alpha;
  return exp(tan2Alpha / roughness2) / denom;
}

float cookTorranceSpecular_1460171947(
  vec3 lightDirection,
  vec3 viewDirection,
  vec3 surfaceNormal,
  float roughness,
  float fresnel) {

  float VdotN = max(dot(viewDirection, surfaceNormal), 0.0);
  float LdotN = max(dot(lightDirection, surfaceNormal), 0.0);

  //Half angle vector
  vec3 H = normalize(lightDirection + viewDirection);

  //Geometric term
  float NdotH = max(dot(surfaceNormal, H), 0.0);
  float VdotH = max(dot(viewDirection, H), 0.000001);
  float LdotH = max(dot(lightDirection, H), 0.000001);
  float G1 = (2.0 * NdotH * VdotN) / VdotH;
  float G2 = (2.0 * NdotH * LdotN) / LdotH;
  float G = min(1.0, min(G1, G2));

  //Distribution term
  float D = beckmannDistribution_2315452051(NdotH, roughness);

  //Fresnel term
  float F = pow(1.0 - VdotN, fresnel);

  //Multiply terms and done
  return  G * F * D / max(3.14159265 * VdotN, 0.000001);
}

vec2 squareFrame_1062606552(vec2 screenSize) {
  vec2 position = 2.0 * (gl_FragCoord.xy / screenSize.xy) - 1.0;
  position.x *= screenSize.x / screenSize.y;
  return position;
}

vec2 squareFrame_1062606552(vec2 screenSize, vec2 coord) {
  vec2 position = 2.0 * (coord.xy / screenSize.xy) - 1.0;
  position.x *= screenSize.x / screenSize.y;
  return position;
}

mat3 calcLookAtMatrix_1535977339(vec3 origin, vec3 target, float roll) {
  vec3 rr = vec3(sin(roll), cos(roll), 0.0);
  vec3 ww = normalize(target - origin);
  vec3 uu = normalize(cross(ww, rr));
  vec3 vv = normalize(cross(uu, ww));

  return mat3(uu, vv, ww);
}

vec3 getRay_870892966(mat3 camMat, vec2 screenPos, float lensLength) {
  return normalize(camMat * vec3(screenPos, lensLength));
}

vec3 getRay_870892966(vec3 origin, vec3 target, vec2 screenPos, float lensLength) {
  mat3 camMat = calcLookAtMatrix_1535977339(origin, target, 0.0);
  return getRay_870892966(camMat, screenPos, lensLength);
}

void orbitCamera_421267681(
  in float camAngle,
  in float camHeight,
  in float camDistance,
  in vec2 screenResolution,
  out vec3 rayOrigin,
  out vec3 rayDirection,
  in vec2 coord
) {
  vec2 screenPos = squareFrame_1062606552(screenResolution, coord);
  vec3 rayTarget = vec3(0.0);

  rayOrigin = vec3(
    camDistance * sin(camAngle),
    camHeight,
    camDistance * cos(camAngle)
  );

  rayDirection = getRay_870892966(rayOrigin, rayTarget, screenPos, 2.0);
}

// Originally sourced from:
// https://iquilezles.org/articles/distfunctions

float sdBox_1117569599(vec3 position, vec3 dimensions) {
  vec3 d = abs(position) - dimensions;

  return min(max(d.x, max(d.y,d.z)), 0.0) + length(max(d, 0.0));
}

highp float random_2281831123(vec2 co)
{
    highp float a = 12.9898;
    highp float b = 78.233;
    highp float c = 43758.5453;
    highp float dt= dot(co.xy ,vec2(a,b));
    highp float sn= mod(dt,3.14);
    return fract(sin(sn) * c);
}

float fogFactorExp2_529295689(
  const float dist,
  const float density
) {
  const float LOG2 = -1.442695;
  float d = density * dist;
  return 1.0 - clamp(exp2(d * d * LOG2), 0.0, 1.0);
}

float intersectPlane(vec3 ro, vec3 rd, vec3 nor, float dist) {
  float denom = dot(rd, nor);
  float t = -(dot(ro, nor) + dist) / denom;

  return t;
}

vec3 n4 = vec3(0.577,0.577,0.577);
vec3 n5 = vec3(-0.577,0.577,0.577);
vec3 n6 = vec3(0.577,-0.577,0.577);
vec3 n7 = vec3(0.577,0.577,-0.577);
vec3 n8 = vec3(0.000,0.357,0.934);
vec3 n9 = vec3(0.000,-0.357,0.934);
vec3 n10 = vec3(0.934,0.000,0.357);
vec3 n11 = vec3(-0.934,0.000,0.357);
vec3 n12 = vec3(0.357,0.934,0.000);
vec3 n13 = vec3(-0.357,0.934,0.000);

float icosahedral(vec3 p, float r) {
  float s = abs(dot(p,n4));
  s = max(s, abs(dot(p,n5)));
  s = max(s, abs(dot(p,n6)));
  s = max(s, abs(dot(p,n7)));
  s = max(s, abs(dot(p,n8)));
  s = max(s, abs(dot(p,n9)));
  s = max(s, abs(dot(p,n10)));
  s = max(s, abs(dot(p,n11)));
  s = max(s, abs(dot(p,n12)));
  s = max(s, abs(dot(p,n13)));
  return s - r;
}

vec2 rotate2D(vec2 p, float a) {
  return p * mat2(cos(a), -sin(a), sin(a),  cos(a));
}

vec2 mapRefract(vec3 p) {
  float d  = icosahedral(p, 1.0);
  float id = 0.0;

  return vec2(d, id);
}

vec2 mapSolid(vec3 p) {
  p.xz = rotate2D(p.xz, iTime * 1.25);
  p.yx = rotate2D(p.yx, iTime * 1.85);
  p.y += sin(iTime) * 0.25;
  p.x += cos(iTime) * 0.25;

  float d = length(p) - 0.25;
  float id = 1.0;
  float pulse = pow(sin(iTime * 2.) * 0.5 + 0.5, 9.0) * 2.;

  d = mix(d, sdBox_1117569599(p, vec3(0.175)), pulse);

  return vec2(d, id);
}

// Source: https://iquilezles.org/articles/palettes
vec3 palette( in float t, in vec3 a, in vec3 b, in vec3 c, in vec3 d ) {
    return a + b*cos( 6.28318*(c*t+d) );
}

vec3 bg(vec3 ro, vec3 rd) {
  vec3 col = 0.1 + (
    palette(clamp((random_2281831123(rd.xz + sin(iTime * 0.1)) * 0.5 + 0.5) * 0.035 - rd.y * 0.5 + 0.35, -1.0, 1.0)
      , vec3(0.5, 0.45, 0.55)
      , vec3(0.5, 0.5, 0.5)
      , vec3(1.05, 1.0, 1.0)
      , vec3(0.275, 0.2, 0.19)
    )
  );

  float t = intersectPlane(ro, rd, vec3(0, 1, 0), 4.);

  if (t > 0.0) {
    vec3 p = ro + rd * t;
    float g = (1.0 - pow(abs(sin(p.x) * cos(p.z)), 0.25));

    col += (1.0 - fogFactorExp2_529295689(t, 0.04)) * g * vec3(5, 4, 2) * 0.075;
  }

  return col;
}

void mainImage(out vec4 fragColor, in vec2 fragCoord) {
  vec3 ro, rd;

  vec2  uv       = squareFrame_1062606552(iResolution.xy, fragCoord);
  float dist     = 4.5;
  float rotation = iMouse.z > 0.0
    ? 6. * iMouse.x / iResolution.x
    : iTime * 0.45;
  float height = iMouse.z > 0.0
    ? 5. * (iMouse.y / iResolution.y * 2.0 - 1.0)
    : -0.2;
    
  orbitCamera_421267681(rotation, height, dist, iResolution.xy, ro, rd, fragCoord);

  vec3 color = bg(ro, rd);
  vec2 t = calcRayIntersection_3975550108(ro, rd);
  if (t.x > -0.5) {
    vec3 pos = ro + rd * t.x;
    vec3 nor = calcNormal_3606979787(pos);
    vec3 ldir1 = normalize(vec3(0.8, 1, 0));
    vec3 ldir2 = normalize(vec3(-0.4, -1.3, 0));
    vec3 lcol1 = vec3(0.6, 0.5, 1.1);
    vec3 lcol2 = vec3(1.4, 0.9, 0.8) * 0.7;

    vec3 ref = refract(rd, nor, 0.97);
    vec2 u = calcRayIntersection_766934105(ro + ref * 0.1, ref);
    if (u.x > -0.5) {
      vec3 pos2 = ro + ref * u.x;
      vec3 nor2 = calcNormal_1245821463(pos2);
      float spec = cookTorranceSpecular_1460171947(ldir1, -ref, nor2, 0.6, 0.95) * 2.;
      float diff1 = 0.05 + max(0., dot(ldir1, nor2));
      float diff2 = max(0., dot(ldir2, nor2));

      color = spec + (diff1 * lcol1 + diff2 * lcol2);
    } else {
      color = bg(ro + ref * 0.1, ref) * 1.1;
    }

    color += color * cookTorranceSpecular_1460171947(ldir1, -rd, nor, 0.2, 0.9) * 2.;
    color += 0.05;
  }

  float vignette = 1.0 - max(0.0, dot(uv * 0.155, uv));

  color.r = smoothstep(0.05, 0.995, color.r);
  color.b = smoothstep(-0.05, 0.95, color.b);
  color.g = smoothstep(-0.1, 0.95, color.g);
  color.b *= vignette;

  fragColor.rgb = color;
  fragColor.a   = clamp(t.x, 0.5, 1.0);
}

- Benefit: the same program runs on different processors with different performance levels.

**Operating system side:**

- **Files** = abstraction of input/output devices.
- **Virtual memory** = abstraction of program memory.
- **Processes** = abstraction of a running program.
- **Virtual machine** = abstraction of the whole computer (OS, CPU, programs).

---

### Summary

**==A computer system consists of hardware and systems software that cooperate to run application programs.==** 

==Information inside the computer is represented as groups of bits that are interpreted in different ways, depending on the context.== 

==Programs are **translated by other programs into different forms, beginning as ASCII text and then translated by compilers and linkers into binary executable files.==**

**==Processors read and interpret binary instructions that are stored in main memory**. Since computers spend most of their time copying data between memory, I/O devices, and the CPU registers, **the storage devices in a system are arranged in a hierarchy, with the CPU registers at the top, followed by multiple levels of hardware cache memories, DRAM main memory, and disk storage.==**  

**==Storage devices that are higher in the hierarchy are faster and more costly per bit than those lower in the hierarchy.==** ==Storage devices that are higher in the hierarchy serve as caches for devices that are lower in the hierarchy. 

***Programmers can optimize the performance of their C programs by understanding and exploiting the memory hierarchy.==*** 

*`Refer to:` [[Storage Devices Hierarchy]] [[Cache Memory]]*

**The operating system kernel serves as an intermediary between the application and the hardware**. I==t provides three fundamental abstractions: (1) Files are abstractions for I/O devices. (2) Virtual memory is an abstraction for both main memory and disks. (3) Processes are abstractions for the processor, main memory, and I/O devices.== 

**Finally, networks provide ways for computer systems to communicate with one another.** ==From the viewpoint of a particular system, the network is just another I/O device.==