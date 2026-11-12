# TCP

<table class="packet-frame">
    <colgroup>
        <col style="width: 40px;"/>
        <col span="32" style="width: 3%;"/>
    </colgroup>
    <tr>
        <td></td>
        <td colspan="8">0 - 7</td>
        <td colspan="8">8 - 15</td>
        <td colspan="8">16 - 23</td>
        <td colspan="8">24 - 31</td>
    </tr>
    <tr>
        <td>0</td>
        <td colspan="16">Source Port</td>
        <td colspan="16">Destination Port</td>
    </tr>
    <tr>
        <td>4</td>
        <td colspan="32">Sequence Number</td>
    </tr>
    <tr>
        <td>8</td>
        <td colspan="32">Acknowledgment Number</td>
    </tr>
    <tr>
        <td>12</td>
        <td colspan="4">Header Size</td>
        <td colspan="4">Reserved</td>
        <td class="v-text">CWR</td>
        <td class="v-text">ECE</td>
        <td class="v-text">URG</td>
        <td class="v-text">ACK</td>
        <td class="v-text">PSH</td>
        <td class="v-text">RST</td>
        <td class="v-text">SYN</td>
        <td class="v-text">FIN</td>
        <td colspan="16">Window</td>
    </tr>
    <tr>
        <td>16</td>
        <td colspan="16">Checksum</td>
        <td colspan="16">Urgent Pointer</td>
    </tr>
    <tr>
        <td>20</td>
        <td colspan="24">Options (0-40 bytes)</td>
        <td colspan="8">Padding (4B alignment)</td>
    </tr>
    <tr>
        <td>20-60</td>
        <td colspan="32">Data</td>
    </tr>
</table>
