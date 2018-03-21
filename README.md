<code>

    .foreach ($obj {!dumpheap -short -type System.Net.ServicePoint}) {!do poi(${$obj}+0x10)} .foreach ($obj {!dumpheap -short -type System.Net.ServicePoint}) {!do poi(${$obj}+0x10)} .foreach ($obj {!dumpheap -short -type System.Net.ServicePoint}) {!do poi(${$obj}+0x10)} .foreach ($obj {!dumpheap -short -type System.Net.ServicePoint}) {!do poi(${$obj}+0x10)} .foreach ($obj {!dumpheap -short -type System.Net.ServicePoint}) {!do poi(${$obj}+0x10)} .foreach ($obj {!dumpheap -short -type System.Net.ServicePoint}) {!do poi(${$obj}+0x10)} .foreach ($obj {!dumpheap -short -type System.Net.ServicePoint}) {!do poi(${$obj}+0x10)} .foreach ($obj {!dumpheap -short -type System.Net.ServicePoint}) {!do poi(${$obj}+0x10)}
    .foreach ($obj {!dumpheap -mt 00007ffd29db11a8 -short}) {!do poi(${$obj}+0x18); !do poi(${$obj}+0x4c)}
    .foreach ($obj {!dumpheap -short -mt 00007ffd255bddd0}) {!do poi(${$obj}+0x8))}

</code>
