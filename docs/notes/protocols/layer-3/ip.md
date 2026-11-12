# IP

## IPv4

<table class="packet-frame">
    <colgroup>
        <col style="width: 40px;"/>
        <col span="32" style="width: 3%;"/>
    </colgroup>
    <tr>
        <td>Octets</td>
        <td colspan="8">0 - 7</td>
        <td colspan="8">8 - 15</td>
        <td colspan="8">16 - 23</td>
        <td colspan="8">24 - 31</td>
    </tr>
    <tr>
        <td>0</td>
        <td colspan="4">Version</td>
        <td colspan="4">IHL</td>
        <td colspan="8">Type of Service</td>
        <td colspan="16">Total Length</td>
    </tr>
    <tr>
        <td>4</td>
        <td colspan="16">Identification</td>
        <td>R</td>
        <td>DF</td>
        <td>MF</td>
        <td colspan="13">Fragment Offset</td>
    </tr>
    <tr>
        <td>8</td>
        <td colspan="8">TTL</td>
        <td colspan="8">Protocol</td>
        <td colspan="16">Checksum</td>
    </tr>
    <tr>
        <td>12</td>
        <td colspan="32">Source Address</td>
    </tr>
    <tr>
        <td>16</td>
        <td colspan="32">Destination Address</td>
    </tr>
    <tr>
        <td>20</td>
        <td colspan="24">Options (0-40 bytes)</td>
        <td colspan="8">Padding (4B alignment)</td>
    </tr>
    <tr>
        <td>24</td>
        <td colspan="32">Data</td>
    </tr>
</table>

## IPv6

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
        <td colspan="4">Version</td>
        <td colspan="8">Traffic Class</td>
        <td colspan="20">Flow Label</td>
    </tr>
    <tr>
        <td colspan="16">Payload Length</td>
        <td colspan="8">Next Header</td>
        <td colspan="8">Hop Limit</td>
    </tr>
    <tr>
        <td colspan="32" rowspan="4">Source Address</td>
    </tr>
    <tr></tr>
    <tr></tr>
    <tr></tr>
    <tr>
        <td colspan="32" rowspan="4">Destination Address</td>
    </tr>
    <tr></tr>
    <tr></tr>
    <tr></tr>
    <tr>
        <td colspan="32">Data</td>
    </tr>
</table>
