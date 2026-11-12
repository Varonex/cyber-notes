# UDP

<div class="packet-frame-wrapper">
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
            <td colspan="16">Datagram Size</td>
            <td colspan="16">Checksum</td>
        </tr>
        <tr>
            <td>8</td>
            <td colspan="32">Data</td>
        </tr>
    </table>
</div>
