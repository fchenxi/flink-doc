---


---

<p>flinkabs<br>
computation.<br>
The mechanism used in Flink is called Asynchronous Barrier Snapshotting (ABS)</p>
<p><img src="https://lh3.googleusercontent.com/Pe2eeqCeJDPg42QbQoIpmwnXX5GclvKQAmB_Qq30DxXHL-P-L0SjSCctj4fEVcCowwRidFD5XJw" alt="flink-abs"></p>
<p>1、中央协调器（JobManager中）周期性的在source端注入barrier（黑色实线）。</p>
<p>2、当source端收到barrier后，立刻做一个快照，即记住当前的offset信息，然后将此barrier广播到所有的输出端。图a)（每个source都会对应一个当前的offset值）。</p>
<p>3、当中间的task收到其中一个输入端的barrier后，立刻阻塞这个channel；这个channel中被阻塞的数据buffer起来；直到task收到所有的input的barrier。图b)（count-2这个task有一个input channel的barrier还未到，因此之前的3个input channel就会被阻塞）。</p>
<p>4、当一个task接到它所有的input端的barrier后，立刻做一个快照，即记录当前这个operator中的状态的值；然后将这个barrier广播到输出端。图c)（收到所有input channel的barrier后，做快照，记录此时operator的状态值，并广播输出这个operator的barrier继续向下游流动）。</p>
<p>5、最后，这个operator解除input channel的阻塞，继续后续的计算。直到最后的sink完成，才算一个完整的检查点完成。</p>

