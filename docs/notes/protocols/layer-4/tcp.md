# TCP

<table class="packet-frame">
    <colgroup>
        <col span="32" style="width: 3.125%;"/>
    </colgroup>
    <tr>
        <td colspan="8">0 - 7</td>
        <td colspan="8">8 - 15</td>
        <td colspan="8">16 - 23</td>
        <td colspan="8">24 - 31</td>
    </tr>
    <tr>
        <td colspan="16">Source Port</td>
        <td colspan="16">Destination Port</td>
    </tr>
    <tr>
        <td colspan="32">Sequence Number</td>
    </tr>
    <tr>
        <td colspan="32">Acknowledgment Number</td>
    </tr>
    <tr>
        <td colspan="4">Header Size</td>
        <td colspan="4">Reserved</td>
        <td>C</td>
        <td>E</td>
        <td>U</td>
        <td>A</td>
        <td>P</td>
        <td>R</td>
        <td>S</td>
        <td>F</td>
        <td colspan="16">Window</td>
    </tr>
    <tr>
        <td colspan="16">Checksum</td>
        <td colspan="16">Urgent Pointer</td>
    </tr>
    <tr>
        <td colspan="24">Options (0-40 bytes)</td>
        <td colspan="8">Padding (4B alignment)</td>
    </tr>
    <tr>
        <td colspan="32">Data</td>
    </tr>
</table>
