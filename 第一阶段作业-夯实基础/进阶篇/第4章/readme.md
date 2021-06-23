## 第四章：

### 作业：

1.读取与写入xml文件：

代码：

实体类Student：

```java
public class Student {
    private String name;
    private String subject;
    private String score;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSubject() {
        return subject;
    }

    public void setSubject(String subject) {
        this.subject = subject;
    }

    public String getScore() {
        return score;
    }

    public void setScore(String score) {
        this.score = score;
    }
}
```

操作类Operate：

```java
import com.google.gson.Gson;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Result;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.File;

class Operate {
    public static void main(String[] args) throws Exception{

        DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder documentBuilder = documentBuilderFactory.newDocumentBuilder();
        Document document = documentBuilder.parse("score.xml");

        NodeList nodeList = document.getChildNodes();
        Student student = new Student();
        for (int i = 0; i < nodeList.getLength(); i++) {
            if (nodeList.item(i).getNodeType() == Node.ELEMENT_NODE) {

                NodeList n = nodeList.item(i).getChildNodes();

                for (int j = 0; j < n.getLength(); j++) {
                    Node meta = n.item(j);

                    switch (meta.getNodeName()) {
                        case "name":
                            student.setName(meta.getTextContent());
                            break;
                        case "subject":
                            student.setSubject(meta.getTextContent());
                            break;
                        case "score":
                            student.setScore(meta.getTextContent());
                            break;
                    }
                }
            }
        }
        System.out.println(new Gson().toJson(student));

        document = documentBuilder.newDocument();
        if (document != null) {
            Element s = document.createElement("Student");

            Element name = document.createElement("name");
            name.appendChild(document.createTextNode(student.getName()));

            Element subject = document.createElement("subject");
            subject.appendChild(document.createTextNode(student.getSubject()));

            Element score = document.createElement("score");
            score.appendChild(document.createTextNode(student.getScore()));

            s.appendChild(name);
            s.appendChild(subject);
            s.appendChild(score);

            TransformerFactory transformerFactory = TransformerFactory.newInstance();
            Transformer transformer = transformerFactory.newTransformer();
            DOMSource source = new DOMSource(s);

            File file = new File("score2.xml");
            Result result = new StreamResult(file);
            transformer.transform(source, result);
        }
    }
}
```

截图：

score.xml：

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAV0AAAD3CAYAAAC+eIeLAAAgAElEQVR4nO3db2xTZ54v8K/ZttgTB017KU5YxM3EIWYvSdCUECmR6jJXBEFg2KRRX0zfLPhKTIkQoahSEFJ50V4hIo3SphdFU67kMG/2zW02WQhgQXcH/CKRwp8RxL2KE8cgmEliOrOdEmfjmXZ69jzHjmM7/v/nOLG/n8oiPn58/OSc+pvn/J5jH43X65UQ4ocffkDRuP5/gNmp7Kzrf32WnfVQ3pSWlibd9i+Dv8IPf3Bm5XV1J/5vVtZD2Tc/P5/1dWqKOnSJQqQSulQcchG667K+RiIiiumlfHeAaLX42f8ey3cXaBW60vkPWV0fR7pERCpi6BIRqShmeeHx48dq9oOIaFXKdhZqJFnkwlzM2K1mT58+xY4dO/LdDSIqAiwvEBGpiKFLRKQihi4RkYoYukREKmLoEhGpiKFLRKSiovsY8Ndffw2dThe2rKSkRFkeamFhARUVFSr2LHM3zl1FC+ohfVSe764QUQxpj3QlyYvpkZu4efMRPCtP9V21FhcX8dJLL4XdROhGEqGbM/b70LxxHzdy9wrZtxb7TLQKpRW6kucRbt16BBjKst0fVbz++usJbwm5emHWdsCW++6uDcr2MKPXle+OEK1uKYeuJHkw/hDY2dyISn0uurQG2Dqgqz0DdJ/G/nz3ZbWo6sRZyxjO1OrQwb9ERDGlXNPVaAyo22dQfl47RYXscfWaUXvmLo4OLqIvSuJOXb6N6ove5QU/lOP6g104EPmY/OeupX442KzxxFsYOeL/Eu1otdmo9VpxyH96NuTVNfJKIzoU2aZl14r1fly5Bx+6b6PluibtPgv7+3wYN8nbp00Hx4Vx2DurVm4goiJXdBNpob766qsVy+J9B4OtQ4tWawO6HYuImidywFV/VioH1h4lsCJtO7IH0hF/O80pBIMtHUoYKq91KLgOfzDHazOPT969Dc258PAevXgHLS3ysnti2Sx++cY9fHy5GgfkQE21z1Wddiwe7IW5pgZa5xB80f4yERWxoj5lTARs5C06F3rNInAtGPLZowfuknWzGLLnorehZvGrz+Zx7NN4ATiP4ZvzaDxZHdKmFO+/txkYngmfEAsb/Zaj9ZAcxG4v0lbVCbtvCBZrK7TmXrDMS7SsqEe6sfz1r3+NWFKFTrsPJjHS1TrkkW6M4DXL4fWFE01vX4Vmnf9QPfIQPCuevMC4/E9t3EZeTEzI4Tl5B5qLkY/l+JQyMalW04UxC0e6RJGKKnS///575d9vvvkGr776atQ24mstnz17FvUxUbN0iJplrQ7OGDVdVJgw8sDk//mJCODbaMKe7AZvxYYEgbvsWM8hfG7O3ksnJCYZ2/qxu9sBH2u6RCsUVXlBnJMrPhgxOzurBG+kv/zlL/j973+vnDK2adOmqOtQapaDR9HfpoM50flRFZvxzvYoy7fq0YiZmGWIqsrSsBKAUqtdmuRS+EsAl37thP8C8qJWu7LNBydLcelUls6tTdBnQdS8ReCKSUZOohFFl/KXmIsPRbhHR+Be0EQsL4GxqRFGvSbGM1cPr9erhG55eXlwxCsuPe92u/HKK69g69atiVeiHEI7cdbXFzxtbMWZC4hdXohsG97OP5l1KVCiEDXXyUonqt2mkNpreBvx/N/gfkSb6H0KreEmfaZEoj4r22MA7bFKL0SkKNorR0QGr7h6hKjlVlZWYt26ojoAICIVFVVNN5Re7/9khyg1iD8y4mO/DFwiyrWiDV1BBK8oK/z5z3/G5s2bsX79+nx3iYgKXNEP6zZs2KD8G+tsBiKibCr60CUiUhNDl4hIRQxdIiIVMXSJiFTE0CUiUhFDl4hIRQxdIiIVMXSJiFTE0CUiUhFDl4hIRQxdIiIVpfWFN8pl2G89hEfj/y5XyVCHfXWGrHaMiKgQpRy6/i8xfwjsbEazQRMM4JGSJjQZ9bnoIxFRwUg5dDUaPYxN+0LuG2AoA+Y8HngrS6DXrP4rRxAR5QtrukREKso4dEV5wTMH6A0GjnKJiBLIKHSX6rtzKIOxsiRbfSIiKlgZhe7z8RFMe0WNtxYGjnKJiBJKO3S90yN4OKdfM5ddJyJaDdIKXRG4I9Ng4BIRpSiN83Q9cE97odFo4B79Eu6Qxwx1e1FnYAgTEcWSxnm6BtTt25e4IRERrcDzdImIVMTQJSJSEUOXiEhFDF0iIhUxdImIVMTQJSJSEUOXiEhFDF0iIhUxdImIVMTQJSJSEUOXiEhFDF0iIhUxdImIVMTQJUqJC71mLcy9rnx3hNaolL/aURBfYj7qXgjelwx12FdnyFqnqEi5emGu6cLdKJd+kqTd6HbY0VmVh36FsvWga8yCIXu+O0JrVVojXb2xCc3Nzcpt7946lM09xCOPlO2+USpEYGk7YMt3P+JR+mhGzEFiVSfsPh8WFxexOH4Bu+WgvTC+qNz3+VQI3CS2oe2KFQ3dp7E/x13JSKLtTHmVhfJCCUr08ujXu5C4KeWGrQO62jPAag8DOVTPWsZwplaHjtX21yGZbSiH2XlrA9oPrvJR7mrezpSF0F3wwOPVw2DgJdjzwdVrhq6tH0cHF2GPGAreOHcVTZfnlX819cP+2xv3cSNyJfb7y4+L27tOTC099sSJJvk5n1y+vfzYUvvIdUWu59zsiv7u7/Nh/MJu9LfpMqqLKr+3The8ac29cC0/qIxYewNtlMdEqIqfo4xk423DsHbXBjBmObtixF3I25myL+3Q9Ty6iVu3buHLUTdgrOMFKvPA1qFFTReUQ/C+GMOz0Yt30IJ6SPcOybddOIYZfCwHRJB4s3+5OfB4oM3EJKrD3sgzOH2zHJNfbEOjeOzXekwG1jVk97eYEmFxCrgeXM9b6HHdixoIVZ12pXyArhpo0xiK+X/vGgwu+ksPi4vj6EYXakKDF1Z0DbRjXH6dhjH5sfMmjC8OwiIvv2KLXFf8bRhoiZ6uMVgOR29UiNuZciPt0DXU7QvUdBth8Izg5iNPNvtFcfln0FutFgwlqnW27IL0UXngTjlaD8kB4fYuP15hwkjw8eU24UrRI4fWtsC9Y+8t/+w3j+Gb82g8WY0DIc95/73NwPDMyhGfoNRvh2CxtoaPUhNRDvEBy1BfSBmgCp3WbjlcB3AtuKIGdFs7sbRpLGeXfw6sKPltKNiuyHFtQYzMLbztTDmT1tkLoTQaPSqNZZh+6IGndhMMUWaeKdvkkLH7YJJHaa1aR4az+vP45N3bOD0Zut/kn1tSWYcXExNyyEzegeZi5GPl0Z4QPFNhzDIEX/whZhQNMEX+vlUm1GAMTpEqSW2LVLahHNDnxQSaI4Oa+VrczpQLGYdukL4ErOqqS9TtHCYzamt1cA4mOjyOJhAEqJYPY5dHVaI2mVIWBBzrOYTPzUk0FPXVtn7slkPMl9Zfiyjh6nLCIYdxe4qrS2obuq5hYExetzXdv2xrdTtTLmQ8kSZJHow/nIPeYICeo1zVKXW7waOZTZhUbVg+jLXfR8v1VPdjOT44WYpLp6JMHkUQNdRkJq1i91XMzAPW1tAJMXkkaumKOsmV3Crjb0NbT/rrDn+hNbSdKWdSHulKkhfu0RG4F5b/hzHsbEaTgYGbN/v7sDhukg8je2Dr7EvhELgU75+vxv97+x40SwFQvQ3XT+jR4k6tC9uO7MEkbqO6fjj8gdBaZ+CUqwsZfshBjE6HoEWbrj+4TDqa4eFzzG1owxWlhpzJofna3M6UGxq3273iUw0bN27MR1/y5unTp9ixY0e+u0GrkDidrGagHQ575EQcUXrWvfzyy1i6/eQnP8l3f4hWEReuDYyhof0gA5eyJnsTaUQFx3+GQ2e+u0EFhd8yRkSkIoYuEZGKGLpERCpi6BIRqYihS0SkIoYuEZGKGLpERCpi6BIRqYihS0SkIoYuEZGKGLpERCpi6BIRqSijL7xZ+m7daa8exqZGXpySiCiBjEa6C+5HmIZe/o+IiJKRduiKy/S4p8XV140MXSKiJKUdus/HH2KuzIhKXo2SiChpaYWu5J3G9FwZdtZuynZ/iIgKWsqhq0yePRJ1hUoYePVfIqKUpD7Sfe7GNIyoY12BiChlKZ8y9twzB82CBqNfhlw7Wgx4R7/EdEklmhoroecImIgoqpRD11C3D80h98VZDOO3plHC83SJiBLiJ9KIiFSU8SXYNRoD6vYZstEXIqKCx5EuEZGKGLpERCpi6BIRqYihS0SkIoYuEZGKGLpERCpi6BIRqYihS0SkIoYuEZGKGLpERCpi6BIRqYihS0SkIoYuEZGKGLqU0I1zV6E5N5vCM2bxyzfk57zrxFTOerW22Dq00HbYcrBmF3rNWph7XVlfM/d7bqT11Y7e6RGMuhfClpVUNqLJyIuxk0rs96E5BVx/sAsH8t0XwdYBbSsw5OvDflVftwddYxYM2avUfNX8WW37PQ1pj3QlQx2am5uDNwZunrl6YdZ2IBdjqdSV4/MHP4f0zyZsy1cXlO1hRg4GgOpJYp/arljR0H1a3aCPifs9GSwvFAJ5lKWrPQOsmjffKlDVibOWMZyp1SEnR/W5lsw+lQPmvLUB7QeLZJSbjDWw3xm6a5yr1wxdWz+ODi7C3rnyzTd1+TY09cPLtzfu40bI49HqdrFqeWHrirae0NcRt2i1PXF4GNomas1wHp+8G76+psvz4X04LT9v3SxaorRZsr/Ph/ELu9HfpltZ8wyMInvF9tPpoDX3wiWCTvwcOboMLF+6KW2Dq/E/X+wDjaYfbSHtotVZg+0j1rOiTZx9Gmx3bQBjlrOI1oT7PcZ+Xw2ePXsmLd0Et9stvXjxIu5t5nc2aWBgIHj7wvY7aebbbxM+b7XeHA6HtBbdOL5eWr/+TenTqRgN7tyT8NN70vU467j+4RUJH87EXabc33U1ZNkLqecXK5+3Yh2/mJAmQ5ZN9v82oj/R1jMjHftp/HUn+7sFTX0qvble3lbHb6xc9uan0lToz9IN6bj8c7CpeCz0eYHHw9alLD4u74vjUsTSkIfXS1qtNuR5/vW8GbHzEu7TiH5EdkPB/e4Xbb+vAmmNdPXGpmAtd+/eOpR5pzEy6oZXkrL9N4Gi8s9Yt1otGPLZo450guRRwZA9Cy/ZsgvSR+WBO6V4/73NwPBM2KgnvnkM35xH48nqkAmQleuZuuzEpe3VmAy+VhbIh5x23xAs1taI0WUDuq2dWNp8lrPLP4c9ty/0AH8/DlvS64Z0dBC+4Lr86xlzBsfMye9TwXYFVlhwOFbtgfs9zn7Pr6xcmLJ2ZxnmHi5AnM/A6TQ1VKHT7oOpQ36Tah3odsR4k5rlN8wXTjS9LR+yrdMoixpPvIWRI6VZ6sc8Jp4AByqSaevFxAQwOnkHmouRjy2/0Vxu+VCxKssTMaKUUNMlH4oPhYRe0k+Ww7AGZ+5qQpbJPx/NZgeFJPfpUp/Oiwk0R/R6L/d7YKWZ7PfcyTh0hQWvF4xb9YnalcNkRm2tDs7BRUT9/6rChJEHJv/PT8Qb8TaasCdLb8BSbK9I7RnHeg7hc3OCRq4XmJLfkFl5A4p6bFs/dssB5Us4fFzRESVwu9ANx+LyKFicc9uajb5FkdQ+dV3DwFgD2q1xfh/u9wz2e25lPJEmeafxaNoLvbESBo0m8RMoq6o67VgcPJrcpEHFZryzPeL5laVhh3liYqTleqL9OItfnpqJOGRMpBwfnCzFpVP34x6aHrBUo3FiEv8UMTmywlY9GjET9xBahGMyE1IJ1ZiWyw7ym7mtP8r2kUdpDfIB/5UszJgn2qe2nq6YE2hRcb+vKmmNdD2PbuKRZ3kHGXY2o87AwM2b/X1YHDfJh1I9sHUun5wvZnyrL3rDmjaeCB/tbDtiwrHP7qGlPjCb3FKPyRNOVLuXn6O8QS/eh+b68rJjn/4cI4lGLhG2HdmDSch9qh8OfyC0bihGaP8CZWSmubj8/9SKw2O53W9OzqL69DAuRWsTOJ3qQtzD9ETkQ35rNwZqWqELBK20+wKGLuxGqzOyaSes3QOolYOyP7Bo94Xx9N/0MfapHCm4YgUsQ7EPl7nfM93vuaURZy0s3dmyZQseP36MjRs35rNPqnv69Cl27NiR724UFGXkhPqQSRjKBnE6Wc1AOxz2KJN+qwD3e2I8T5eyQD7sDD3v8okTH8uDmmN7+cbLLheuDYyhof3gKglc7vd0ZGUijYpdOT7fK05+vx9cIg5DE06cUIr8Zzh05rsbQdzv6WDoUnaI05Tu5bsTpDru95SxvEBEpCKGLhGRihi6REQqYugSEamIoUtEpCKGLhGRihi6REQqYugSEamIoUtEpCKGLhGRihi6REQqSvu7FyTJC/foCNwL/u++LKlsRJORV48gIoonrdCVJA/Gbz2EV1ygkkFLRJS0tMoLz8cfYq5sJ0e2REQpSjl0RVlBXIeyzLApF/0hIipoaZQXFrDg1aMEbozcnMaCRiMHcQmMTY0w6nmdNCKieFIP3QV5pCv/NzdtQFNzM/Ry6HqnRzAyMg59cy2vCExEFEfqNd0SPcR/xrpKJXCVRZVGlGEOnufZ7h4RUWFJ8zxdLxYWIpfpUVKSaXeIiApbyqGr0RhQadRjbtoNr+S/ertyNoPeAANDl4gorrTO09Ubm7Bz4SZGv3Qr96USI5oal8sNREQUXdqfSDPU7UNzNntCRFQE+N0LREQqYugSEamIoUtEpCKGLhGRihi6REQqYugSEamIoUtEpCKGLhGRihi6REQqYugSEamIoUtEpCKGLhGRihi6REQqYugSpcSFXrMW5l5XvjtCa1TKX+0oeacxOuK/IGUkQ91e1Bn4nbqUe7YOLdr6l/9fk3ZfgMPeiarwRtC19Qfv7r4wDntnWIs0XrgHXWMWDNkzXA8VrdSvHKE3omnfPjQ3NwdvexsrUSKV8HI9+eTqhVnbAVu++5Ep5fcwI95AUgRuq6Mb44uLWFRu4+hGF2o6bKGNoG114MJ4oM34BaCrJv4INYltaLtiRUP3aexP+RdbZZLYzpQbWSkvPHdPw1tm5CXY80WM6GrPAIUQBlWdOGsZw5laHTqipp8LTof8T40pZFRbhYPtDYDDCVegTe95EY5WBAe28nqt3Q0Y6+qJHqrJbEM5qM5bG9B+sABGuQm3M+VK2leOWCLKDdNzQNnOTdnoD6XI1WtG7Zm7ODq4iL4oaTF1+TaqL3qXF/xQjusPduFAWKt5fPLubZyeXP6j2XjiLYwcKY29nuptmPxnE7Yt3bffh+bXennZBvzqjXu4tM6/rmM9h/C5OeSlRLvTs8v3W3ZB+qg8rDf7+3wYN8m/V5sOjhUlgSp0nrXgTFsbtBiET/zScmCKbWAZtPuD2HUNA2NyOFpDnicHpqVrTPnRKSfz/rCH4m/DYLtrAxiznEW0ykLhbWfKmWfPnklLN8HtdksvXrxI+jbzO5v0he130sy336b0vNV0czgc0lp04/h6af36N6VPp2I0uHNPwk/vSdfjrmVGOvbTKxI+nInZ4vqHVyLW80Lq+YW87BcT0mToa+26Krf7rdTz2L9osv+3Yc+LvB9cT6zXnvpUenO9/DsevxHlwRvScfkxrVYrb4PjUliLG8fDtktwO93wry90dQm3YcTrRe1KQW9nyraMyguS5IF72osyIy9KqS7/DHqr1YIhnx1xByjrZjFkj/3w1GUnLm2vxmTEKCjoiRMfD8sjqU9DR22leP98NRonZjH8JKTtD3r0/MsevF/hv7ttTzka5dHdhNJmHsM359F4sjp8Pe9tBoZncCPaa8uHwHbfECzWVmjNvQiWH5V6ZCsc3Q6lXjtksaJNF+UwWWknbycMwbdiO6WwDQXbFVhhweFYI+FC3M6UExmVFxbc05gTE2usLKhMPsS2+2ASE0paB7odMULDLB9SfuFE09tXoQkchkYezrrc8/LqQg5foyrF9q0Riyo2oBaTmHgqfg5pVxHaxoSRB6bAHS8mJoDRyTvQXIxcf4wgEqFZ0yUf0g/5ywj+hei1BJYFfmlxmDwIeVu0duCwrw/75d+nQY7IrlbI28YXUQ5ogEm5n+Q2XHpNpUbsiF7vLcjtTLmSdugujXL1xjqOcvNEhI1D1ORqdXDGqkeGviGfiGC4jSbsCQsEuF5gSn5Dxg6E+Yg3vVjXC4zLb/53IkMigRW1x1gCp3vtloPOF5aGLjjH5OhsD0/I/YctkHPWTw63GvFv6EQaAjXZhnaElnqT2obRasSRCm47U66kX1547sEcymCs5Hli+VTVacfi4FH0t+kSn7BfsRnvbA9fdMAiDl8n8U+X52M8x4QPDwGXTt0POTSdxydnJzF6yBQ8xE2sHB+cLI1YT3TilDARBGJia+XkThVMDYg4C8E/EkXD0hkN+3FaOVPBsnxKVGAizXI24lxeJN6Gtp4uZQIt6UwqiO1MuaIRE2hLd7Zs2YLHjx9j48aNcZ8kRrnjtx7Ca2xCk1Gf807m2tOnT7Fjx458dyMzyiGiE2fF4XVg0YqZcKw87FUoIzP5zb0u9qz6jXNX0XI95IgmcjZczJafQpQZ+3DR+hS2LuX3GEB7osN9cw3O3A35cMTRwRWHxktnJSxJdHZCtG0oRxM6tK3AkC/mcwt3O1MupBW6haYgQpdyQgR3zUD7yk+7EaWJ371AFJML1wbG0NB+kIFLWZPxhyOICpf/DIfOfHeDCgpHukREKmLoEhGpiKFLRKQihi4RkYpiTqT9SPudmv3Iu7999x9Rl//dy6/FfIyIKFUc6RIRqYihS0SkIoZuEl565b8pNyKiTDF0iYhUxNAlIlIRQ5eISEUMXSIiFaX1hTeS9DXuX3XgD4ErRkjSj7D9Z7uxvZRXkCAiiifl0JWkBTjvOPDC1IB/NPmvGjHvHMO/3X+Cv3+rAqW8dA8RUUxplBf+E94XwIYNPwou0W/QAy8WEeNCJEREFJBy6Go0r8Mkj3B/f/cuJuYlf6nhrgcbTP8dmznKJSKKK62abqkoLWz4/7hy+w6c8v2/370H/7OcgUtElEhaZy+IGu6/3vXCtOctHK7fhD/cu4N/dy5ku29ERAUn5dAV5QSnHLBbdvvPVtCU/w8leF84v1LKDUREFFvqI12vFy9QAn3oldf1OmzIXp+IiApW6qErp+0GLGBi8o/BRbOTT+Qglpfr4zyPiIhSn0gTZy/s+nkNcNWBf51Z+nCEAQ0//weevUBElEBaZy+I4K0//DPUZ7s3REQFjt+9QESkIoYuEZGKGLpERCpi6BIRqSitibRi8/1f/5TvLhBRgeBIl4hIRTFHuv/pe1nNfuTd3738WlqPERGlgiNdIiIVMXSJiFTE0CUiUhFDl4hIRUUfut98843y79dff53nnhBRMSja0JUkCX/84x+xsLCA1157TQndpQAmIsqVogzd7777Dh6PB3/7299QWVmJ8vJy5TY7O5vl4HWh16yFudeVxXVm5sa5q9Ccm813N4iKVlqfSJMkL9yjI3AvBL5P11CHfXWGrHYsV3w+nzLCLSkpwZYtW7Bunf/vzquvvqr8K4I39H5GbD3oGrNgyF6V+nPt96E5BVx/sAsHMu+JOtZin4lUlsY10vyB6zE0obm5GXv3NsLofYiRaW8u+pdV33//vVJG2LhxI7Zu3RoM3CUiaJdGvHNzc/FX5uqFWdsBW5wmtitWNHSfxv7Mu14YlG1mxioa+BOpLvXywoIHHm8ZjJUlyl2NRo9KYxm80254pNV9YcqXXnpJqeW+/vrrMduI4K2qqsKf/hTn+xZsHdDVngHiBaocMOetDWg/mMYot1BVdeKsZQxnanXoiPfXiqiApV5eWFjAijGtfKiuhwfeBcBQANdJe+WVV2I+5uo1o/bMXRwdXERfnCGs69oAxixnEa2yMHX5NqovhmzFH8qDh+Rhj8l/Elvqh4PNGk+8hZEjpcrPojbbgnpIH5UHH4+2TDnkPx1aw9XIK43oUGSbll0r1vtx5R586L6NluuatPss7O/zYdwkb8M2HRwXxmHv5B8lKi6ph+4mA8rwENPuShiMen+54dG0HMR6rI2q7rKvvvpqxbIdO3bEbG/r0KJVHr12OxYRPyts6Okag2UoSirLAVf9WakcWHui1j23HdkD6QiyUh9VwlB5rUPBdfiDOV6beXzy7m1ozoWH9+jFO2hpkZfdE8tm8cs37uHjy9U4IAdqqn2u6rRj8WAvzDU10DqH4Iv314uowKRcXtBoDKhtMgLTI7h165Z8ewQYjViLA1wRsJG36PxnIbRaLRjy2RMELkQxF1ZYcDhWlqybxZA9g44nZRa/+mwexz6NF4DzGL45j8aT1SFtSvH+e5uB4RncCG0aNvotR+shOYjdGdTxqzph9w3BYm2F1twLlnmpWKR3YUq9EU37jMH7kucRpuXYNZZkrV+rTBU67T6YxEhX65BHuvGCVw7o82ICzRG93muWw+sLJ5revgrNOv+heuQheFY8eYFx+Z/auI28mJiQw3PyDjQXIx8rj/aE7BGTajVdGLNwpEvFJStfYv7cMweU7YShwC/BLuqRDlGPrNXBGaum67qGgbEGtFvjDIcrTBh5YPL//EQE8G00YU92g7diQ4LAXXas5xA+N2fvpRMSE5Ft/dgt/2HysaZLRSbjD0d4Ht3Ew7ky7KzdlI3+rHpKPXLwKPrbdFE/9GDr6VIm0JLOkorNeGd7lOVb9WjETMwyRFVlaVgJQKnVXg/9o+cvAVz6tRNTyn1Rq13Z5oOTpbh06n54KSFdCfosiLq4CFwxEclJNCpGaY10RdA+8ix9MGIn9u1bW1NoMzMzYffFR4F//OMfJ7+C/X1YHDfJh8c9sHX2hZQRbLhiRfQJtIAVZy5AlBeijHLl0fBvTs6i+vQwLgXbLZchth0x4dhn99BSHzjroKUekyecqHYvr+LAR/U49sY9VNdPBV9nct/9sDZiEmwScp9Czjjwry/8DIakJOjz0ml0F+KWZ4gKm+bZs2fBk2vFJ7QeP36sfHigUIDq49UAAADBSURBVD1//hzr168PW/btt99i06bwkboI4oqKipTWLU4nqxloh8PeCWYKEUVTdBemjAxXQYRu5Acm4n2AIjoXrg2MoaHdysAlopiKLnRzx3+GQ2e+u0FEq1pRfssYEVG+MHSJiFTE0CUiUhFDl4hIRQxdIiIVMXSJiFTE0CUiUhFDl4hIRQxdIiIVMXSJiFTE0CUiUhFDl4hIRQxdIiIVMXSJiFTE0CUiUhFDl4hIRQxdIiIVMXSJiFTE0CUiUtF/AYobQmxwzRR2AAAAAElFTkSuQmCC)

运行后的json输出截图：

![](data:image/png;base64,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)

score2.xml：

![](data:image/png;base64,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61CuUcGS+E6EEWGJiIiIiIiIKLD4tGHhyvfDcOPaL07DiTAiLBEREREREREFFp81LNy4eh6l577RHF6EFesQERERERERUeDwXcOCPLRB7zScmZ7DIYiIiIiIiIgCjM8aFnRBNfyyDhERERERERFVHJ81LFSr0QDQubB5Kay8DhEREREREREFDN/1WLj1DgTdGas5fNCdf5bXISIiIiIiIqLA4dO3QlS/f6z2sC1e92FMiIiIiIiIiMgXfNqwcGuj5xEc/pTzcA164daGvX0ZFSIiIiIiIiLyAZ82LAg12n0s/Vdn93vdLSHGMEREREREREQUaIJ8vYNqIfVwW+MXcONKvu3v/9QMutvCfB0NIiIiIiIiIvIBnzcsCDUeSfPHboiIiIiIiIjIz/zSsLBgwQLk5OTY/C4qKgojR470RzSIiIiIiIiIyMv80rCQlJSEq1ev2vwuJCTEH1EgIiIiIiIiIh/wS8PCXXfdhV27dtn8rkuXLv6IAhERERERERH5gF8aFoKDg6HX68sNhxDDIMR3RERERERERBSY/NKwIDzxxBP+2hURERERERER+Um1io4AEREREREREQUuNiwQERERERERkdvYsEBEREREREREbmPDAhERERERERG5jQ0LREREREREROQ2NiwQERERERERkdvYsEBEREREREREbmPDAhERERERERG5jQ0LREREREREROQ2NiwQERERERERkdvYsEBEREREREREbmPDAhERERERERG5jQ0LREREREREROQ2NiwQERERERERkdvYsEBEREREREREbmPDAhERERERERG5jQ0LREREREREROQ2NiwQERERERERkdvYsEBEREREREREbmPDAhERERERERG57f8B5v5RIWAzP64AAAAASUVORK5CYII=)



### 笔记：

#### XML简介：

+ 为什么需要XML
  + 数据是主体
  + 但是，单独的数据，它的含义很模糊
  + 数据+含义，适用于传输数据，而不是显示数据(HTML)

+ XML基本概念
  + XML(eXtensible Markup Language)，www.w3.org
  + 可扩展标记语言:意义+数据
  + 标签可自行定义，具有自我描述性
  + 纯文本表示，跨系统/平台/语言
  + W3C标准(1998年，W3C发布了XML1.0，包括几乎所有的Unicode字符)

+ XML结构常规语法
  + 任何的起始标签都必须有一个结束标签。
  + 简化写法，例如，<name></name>可以写为<name/>。
  + 大小写敏感，如<name>和<Name>不一样。
  + 每个文件都要有一个根元素。
  + 标签必须按合适的顺序进行嵌套，不可错位。
  + 所有的特性都必须有值，且在值的周围加上引号。
  + 需要转义字符，如“< ”需要用& lt;代替。
  + 注释:<!--注释内容-->

+ XML扩展
  + DTD(Document 'Type Definition)一定义XML文档的结构
  + 使用一系列合法的元素来定义文档结构
  + 可嵌套在xml文档中，或者在xml中引用
  + XML Schema(XSD，XML Schema Definition)
    + 定义XML文档的结构，DTD的继任者
    + 支持数据类型，可扩展，功能更完善、强大
    + 采用xml编写
  + XSL
    + 扩展样式表语言(eXtensible Stylesheet Language)
    + XSL作用于XML，等同于CSS作用于HTML
    + 内容
      + XSLT：转换XML，文档
      + XPath：在XML文档中导航
      + XSL-FO：格式化XML文档
    + 可进一步访问http://www.w3school.com.cn/x.asp进行学习

+ 总结
  + 了解XML的基础概念
  + 了解XML的多个分支技术



#### XML解析（DOM方法）：

+ XML解析
  + XML解析方法
    + 树结构
      + DOM：Document Object Model文档对象模型，擅长(小规模)读/写
    + 流结构
      + SAX：Simple API for XML流机制解释器(推模式)，擅长读

      + Stax：The Streaming APl for XML流机制解释器(拉模式)，擅长读，JDK6引入

+ DOM
  + DOM是W3C处理XML的标准API
    + 直观易用。
    + 其处理方式是将XML整个作为类似树结构的方式读入内存中以便操作及解析，方便修改。
    + 解析大数据量的XML文件，会遇到内存泄露及程序崩溃的风险。

+ DOM类
  + DocumentBuilder 解析类，parse方法
  + Node节点主接口，getChildNodes返回一个NodeList
  + NodeList节点列表，每个元素是一个Node
  + Document 文档根节点
  + Element标签节点元素(每一个标签都是标签节点)- Text节点(包含在XML元素内的，都算Text节点)
  + Attr节点(每个属性节点)



#### XML解析（SAX方法）：

+ SAX
  + Simple APl for XML
  + 采用事件/流模型来解析XML文档，更快速、更轻量。
  + 有选择的解析和访问，不像DOM 加载整个文档，内存要求较低。
  + SAX对XML文档的解析为一次性读取，不创建/不存储文档对象，很难同时访问文档中的多处数据。
  + 推模型。当它每发现一个节点就引发一个事件，而我们需要编写这些事件的处理程序。关键类：DefaultHandler



#### XML解析（Stax方法）：

+ Stax
  + Streaming API for XML
    + 流模型中的拉模型
    + 在遍历文档时，会把感兴趣的部分从读取器中拉出，不需要引发事件，允许我们选择性地处理节点。这大大提高了灵活性，以及整体效率。
    + 两套处理API
      + 基于指针的API，XMLStreamReadcr
      + 基于迭代器的API，XMLEventReader

+ 其他的第三方库
  + DOM/SAX/Stax是JDK自带的解析功能。
  + 第三方库
    + JDOM：www.jdom.org
    + DOM4J：dom4j.github.io
  + 第三方库一般都包含DOM,SAX等多种方式解析，是对Java解析进行封装。

+ 总结
  + DOM：读(小规模)XML，写XML
  + SAX/Stax：适合读(大规模)XML(一般大于1M)



#### JSON简介及解析：

+ JSON概念
  + JSON
    + JavaScript Object Notation，JS对象表示法
    + 是一种轻量级的数据交换格式
    + 类似XML，更小、更快、更易解析
    + 最早用于Javascript中，容易解析，最后推广到全语言
    + 尽管使用Javascript语法，但是独立于编程语言

+ JSONObject和JSONArray
  + 名称/值对。如"firstName":"John"
    + JSON对象: {"name":"Jo" ,"email": "a@b.com"}
    + 数据在键值对中
    + 数据由逗号分隔
    + 花括号保存对象
  + JSON数组
    + 方括号保存数组
    + [i"name":"Jo" ,"email" :"a@b.com"}, i"name" :"Jo" ,"email" :"a@b.com"}]

+ Java的JSON处理
  + org.json：JSON官方推荐的解析类
    + 简单易用，通用性强
    + 复杂功能欠缺
  + GSON: Google出品
    + 基于反射，可以实现JSON对象、JSON字符串和Java对象互转 
  + Jackson：号称最快的JSON处理器
    + 简单易用，社区更新和发布速度比较快

+ SON主要用途
  + JSON生成
  + JSON解析
  + JSON校验
  + 和Java Bean对象进行互解析
    + 具有一个无参的构造函数
    + 可以包括多个属性，所有属性都是private
    + 每个属性都有相应的Getter/Setter方法
    + Java Bean用于封装数据，又可称为POJO(Plain Old Java Object)

+ JSON和XML比较
  + 都是数据交换格式，可读性强，可扩展性高
  + 大部分的情况下，JSON更具优势（编码简单，转换方便）而且JSON字符长度一般小于
  + XML更加注重标签和顺序
  + JSON会丢失信息

+ 总结
  + JSON是一种独立于编程语言的、轻量的、数据交换格式
  + 有多种第三方库辅助我们进行JSON生成和解析
  + 注意：JSON会丢失顺序性。



## 第四章（续）：

### 作业：

1.读取和写入文件：

代码：

```java
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.URL;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

import com.lowagie.text.Font;
import com.lowagie.text.pdf.BaseFont;
import fr.opensagres.xdocreport.itext.extension.font.IFontProvider;
import fr.opensagres.xdocreport.itext.extension.font.ITextFontRegistry;
import org.apache.poi.util.Units;
import org.apache.poi.xssf.usermodel.XSSFCell;
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.apache.poi.xwpf.converter.pdf.PdfConverter;
import org.apache.poi.xwpf.converter.pdf.PdfOptions;
import org.apache.poi.xwpf.usermodel.*;
import org.krysalis.barcode4j.impl.code39.Code39Bean;
import org.krysalis.barcode4j.output.bitmap.BitmapCanvasProvider;
import org.krysalis.barcode4j.tools.UnitConv;

import static org.apache.poi.ss.usermodel.CellType.NUMERIC;
import static org.apache.poi.ss.usermodel.CellType.STRING;

class ReadXandDtoP {

    public static void main(String[] args)throws Exception {
        List<String> list;
        list = readExcel();
        String path = "student.png";
        generateBarcode(list.get(2), path);

        XWPFDocument doc = openDocx(
                "C:\\Users\\yhf\\Desktop\\student.docx");

        Map<String,Object> params = new HashMap<>();
        params.put("{name}", list.get(0));

        params.put("{sex}", list.get(1));

        Map<String,String> picParams = new HashMap<>();

        picParams.put("barcode", "student.png");

        List<IBodyElement> ibs = doc.getBodyElements();

        for(IBodyElement ib : ibs) {
            if(ib.getElementType() == BodyElementType.TABLE) {
                replaceTable(ib, params);
            }else {
                replaceBarcode(ib, picParams, doc);
            }
        }

        writeDocx(doc, new FileOutputStream("student1.docx"));

        PdfOptions options = PdfOptions.create();
        options.fontProvider(new IFontProvider() {
            public Font getFont(String familyName, String encoding, float size, int style, Color color) {
                try {
                    BaseFont bfChinese = BaseFont.createFont(
                            "C:\\Windows\\Fonts\\STSONG.TTF",
                            BaseFont.IDENTITY_H,BaseFont.EMBEDDED);
                    Font fontChinese = new Font(bfChinese, size, style, color);
                    if(familyName != null) {
                        fontChinese.setFamily(familyName);
                    }

                    return fontChinese;
                }catch(Throwable e) {
                    e.printStackTrace();
                    return ITextFontRegistry.getRegistry().getFont(familyName, encoding, size, style, color);
                }
            }
        });
        PdfConverter.getInstance().convert(doc, new FileOutputStream("student1.pdf"), options);
    }

    public static List<String> readExcel() throws IOException {
        List<String> msg;
        msg = new ArrayList<String>();
        InputStream stuExcel = new FileInputStream(
                "C:\\Users\\yhf\\Desktop\\student.xlsx");
        XSSFWorkbook wb = new XSSFWorkbook(stuExcel);
        boolean stuFlag = false;

        XSSFSheet sheet = wb.getSheetAt(0);
        XSSFRow row;
        XSSFCell cell;

        Iterator rows = sheet.rowIterator();

        while(rows.hasNext()) {
            row = (XSSFRow)rows.next();
            Iterator cells = row.cellIterator();

            while(cells.hasNext()) {
                stuFlag = true;
                cell = (XSSFCell)cells.next();
                if(cell.getCellType() == STRING) {
                    msg.add(cell.getStringCellValue());
                } else if (cell.getCellType() == NUMERIC) {
                    msg.add("" + cell.getNumericCellValue());
                }
            }
        }
        if(stuFlag) {
            return msg;
        }else {
            return null;
        }
    }

    public static void generateBarcode(String msg, String path) {
        File file = new File(path);
        try {
            Code39Bean bean = new Code39Bean();

            final int dpi = 150;
            final double width = UnitConv.in2mm(2.0f / dpi);
            bean.setWideFactor(3);
            bean.setModuleWidth(width);
            bean.doQuietZone(false);

            String format = "image/png";
            BitmapCanvasProvider canvas = new BitmapCanvasProvider(new FileOutputStream(file), format, dpi,
                    BufferedImage.TYPE_BYTE_BINARY, false, 0);

            bean.generateBarcode(canvas, msg);

            canvas.finish();
        }catch(Exception e) {
            e.printStackTrace();
        }
    }

    public static XWPFDocument openDocx(String url) {
        InputStream in = null;
        try {
            in = new FileInputStream(url);
            XWPFDocument doc = new XWPFDocument(in);
            return doc;
        }catch(FileNotFoundException e) {
            e.printStackTrace();
        }catch(IOException e) {
            e.printStackTrace();
        }finally {
            if(in != null) {
                try {
                    in.close();
                }catch(IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return null;
    }

    public static void writeDocx(XWPFDocument outDoc, OutputStream out) {
        try {
            outDoc.write(out);
            out.flush();
            if(out!=null) {
                try {
                    out.close();
                }catch(IOException e) {
                    e.printStackTrace();
                }
            }
        }catch(IOException e) {
            e.printStackTrace();
        }
    }

    private static Matcher matcher(String str) {
        Pattern pattern = Pattern.compile("\\{(.+?)\\}", Pattern.CASE_INSENSITIVE);
        Matcher matcher = pattern.matcher(str);
        return matcher;
    }

    public static void replacePic(XWPFRun run, String imgFile)throws Exception {
        int format = Document.PICTURE_TYPE_PNG;

        if(imgFile.startsWith("http") || imgFile.startsWith("https")) {
            run.addPicture(new URL(imgFile).openConnection().getInputStream(),
                    format, "rpic", Units.toEMU(100), Units.toEMU(100));
        }else {
            run.addPicture(new FileInputStream(imgFile), format,
                    "rpic", Units.toEMU(250), Units.toEMU(80));
        }
    }

    public static void replaceTable(IBodyElement para, Map<String, Object> params)throws Exception{
        Matcher matcher;
        XWPFTable table;
        List<XWPFTableRow> rows;
        List<XWPFTableCell> cells;

        table = (XWPFTable)para;
        rows = table.getRows();

        for(XWPFTableRow row : rows) {
            cells = row.getTableCells();

            for (XWPFTableCell cell : cells) {
                String runText;
                List<XWPFParagraph> ps = cell.getParagraphs();

                for (XWPFParagraph p : ps) {
                    for (XWPFRun run : p.getRuns()) {

                        runText = run.text();
                        matcher = matcher(runText);

                        if (matcher.find()) {
                            if (params != null) {
                                for (String picKey : params.keySet()) {
                                    if (matcher.group().equals(picKey)) {
                                        run.setText(params.get(picKey) + "", 0);
                                    }
                                }
                            }
                        }
                    }
                }

            }
        }
    }

    public static void replaceBarcode(IBodyElement para,
                                      Map<String, String> picParams, XWPFDocument indoc) throws Exception{
        XWPFParagraph p = (XWPFParagraph)para;
        String runtext;

        for(XWPFRun run : p.getRuns()) {
            runtext = run.text();
            if(picParams != null) {
                for(String pickey : picParams.keySet()) {
                    if(runtext.equals(pickey)) {
                        run.setText("",0);
                        replacePic(run, picParams.get(pickey), indoc);
                    }
                }
            }
        }
    }
}
```



截图：

docx文件：

![](data:image/png;base64,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aturbY1x1bCYvnU5Ha1P6/bPbVtbt7lx3nN7n1vF66rtjOlb3+fW9Z3b+9yo8X1ufB8b7xs/99b1qZbxTt+z1uW1rlfj9rd+l97pe1bNO5YVy4/5VJ976/e69Xnj8t7tc63WofV732hu2/9u2/pOn0frd7r1e/Zu29r4N9W6Hu8272oeHW1XDGtd79bltY6P5XXm7weArrdABOttt902rbbaak3DlllmmTx88cUXTyuuuGJ+XIlh4Ytf/GJ67rnn6sPXX3/9fP+1r30trbvuuumFF15oOjAPGjQo348YMSKtvfbaTcvr3bt3XsZSSy2VFltssfx40UUXTauuumrTsj/zmc+kz372s3lYr1690lprrdU0fpVVVmm3fdtvv31ab7316tvav3//9LGPfaxpuvDpT3867bDDDk0H4z59+uQDd+NrY70+8pGPNE276aabppdeeik//upXv5o+97nP1bd1jTXWSP/973/TEksskYftuOOOaeDAgU3Tx3vauj4f//jH00orrTTXbY33IWy22WZpueWWqw+v5h2v/fznP980z+pzbRTbEuu43Xbb5eeDBw/O2x3vU/Xa+Dwbp4t1aRXvc3zmlVj3T3ziE/XPtXr8qU99Kt/HOi+yyCL5cXz+jeL7Ee9dtT6zZ8+ujxsyZEi+j/f5qaeeqg+P9d1iiy1Sv3796sNiu2L7YhmxPo1WWGGFPDy+C43bGqr3c5NNNsnfh9je+NzCl770paYQu+GGGzbNd+GFF87ziu9ufFda3+94H6rPNb5zcb/kkkum5ZdfPj+O73+j+Lt6+umn688HDBiQ77faaqv0j3/8Y67bOnz48PTPf/6zPj6+L9XfVnwGjaq/s/ierbPOOvVt3WCDDZoCcLWt8XdSfc/ic2p8P2I7GlV/rwsttFB+H6vPNfYHL774Yv11saz4HlR/R41i+livEN+zxtfE/qmjv5/4brV+rgB0rV61jkohAADAPFkgKtb0PFHJjUprVGUbPfroo7kCH1XVxspuNU1U1qMCGFXcjsyaNStXO6OaHmJeMc9GscyoFjZOE6Ji/8lPfjI/jsr2s88+227+yy67bK4+xnpU1ej4leKjH/1o/TXRbOKRRx7Jj6MCH9Xe1nUMURlurORXopnDv/71r1xJjQptJarCVSW0o18+AID3qQbdzP33319rC421oUOHNg2fNm1abaGFFopfYGptwbf2xz/+sT7ub3/7W60t9OZxI0eOrF1yySXt5nvGGWfk8Zdffnl92BNPPJGHNd7+/Oc/18effPLJ9eErr7xy7Sc/+UmtLdjWTj311HbTxS2WG+M333zz+rDddtut1hbg6/Pcd9996+M22GCD2r///e/6uJh/Na4tkNfuvPPOpm2YPXt2bfjw4bW2fyxqb7zxRn34fffdV+vfv3992gkTJryHdx4AeCd6BaFbiUputCl9+OGHm9rqXn311bkN9eqrr57agnGuAu+0007phhtuSE8++WTaZZdd0rhx4/K4aLd70EEHtZv38ccfn+8bq7xRsY7n3/nOd9LUqVPTVVddVW9/HeO+//3v5/a4Md/Ro0en/fffP1eLo51tDIvbb3/723T66aenjTbaKLdVr9ocV+P//ve/pzFjxuR5HnzwwelPf/pTfVy0ZY+2xOG8885LBx54YNpmm23yuOeffz63x33ooYfq6xPbOWXKlHp79/DYY4+lLbfcMlfeY7pY329/+9upLaSX/XAAYEHX1cke5kVUqn/wgx/kim9bUK0Pv/TSS2tDhgypPfXUU/n5rFmzcmX2F7/4Ra7cPv300/XXRkV4qaWWqj9vC6i13Xffvbb22mvnaa644or6uAEDBuSKcls4rd1+++25IlzZe++9a/369Wtav5jXm2++2TQs1mngwIG1E088MT+PZYwfP74+fo899qj17ds3P4512Hrrrevjzj333Fr1Zzp27NhaWyCvvfbaa/l5VOhj3M0335yfjxo1qrbjjjvWjjvuuKaKdbwXgwYNql133XX5eQxfbrnlal//+tc78Y4DAJ2ljTXdRtv3NbWF07TrrrvmCnWt4bzbqE7HrRK9dFSivXW0fY5u00444YQ0Y8aMdPjhh9fH33LLLemXv/xl+v3vf5973WgU7ZXvvPPO9JWvfCVXhqOnmKhAR1vpaK8cbbFvvfXW9Otf/zqtueaaqS2stlvvWOd77rknV71jnQ844IB05ZVX5opzmDlzZjryyCPz46iOV925RU8hF154Yb39ddxHrxhV+/CqKh29UYSRI0fmqv0pp5zS9N5EW+tYRuP7EctprMwDAO+fYE23EUEwQvW7ie7Toju1aI7R2BVZ9A982mmn5YB6++235xMIIxwfeuihafz48fUuEqMLucrSSy+dQ3QE35jujDPOSF/+8pdzUI7XRdOUaI7xzDPP5KAb3bLtvvvu9e4KI7Rfcskl6YILLqhvQwTgiRMnpptvvjkPi2liWDj33HPzukdIj5MzY/4RhDvS2jd21c1baz/LrSLgxwmU0YwFAChHG2t6lKgwRxviqOL+5je/aapc9+3bN1eBo2/yqDpHJflb3/pWnib6OY6qc4jK9R/+8If8+C9/+Uu64oorct/OUa2OfoPvvffePC6qwrGcaJsdoT1C8Te/+c3c/rsSvYNE7x9f+MIX8vMIvcOGDUtHHXVUniZu0S46hoVYxqWXXprbVsc/BRMmTOjwgiLvVfyDEO/Lddddl1ZeeeVi8wUABGt6kLPPPjs3B4lKcpxoWImqb1SR4+S9qglEVIGjUhz30cQjposmGuGHP/xhOuyww/LjOMnvnHPOqc+rsYlFXBwourvbc8898zyrSnfja6pqc+NFSOJxY3OMCOfV+KhCxwVj4iTDCNWNVenWK/BVTUBqneiK/tprr03f+MY38nynTZvWrptCAOD90xSEHiHaK0+aNCk9/vjjueeOBx54IIfSCJDR3OOiiy7K7aSrqxpGM4to5hEV3DPPPDMPi94zhg4dmn7+85/nq2uG6FUkbtGjR/Wa6AM6wmxMF72PRE8e++23X7r//vvzPKs+riMsRzU6ml40XiEypo/5RHOSEFcPjEp4iF4/IuifeuqpeVuish3bFaL/7f/85z9pn332SYccckgaNWpUvoJk65UMW8U/APGenH/++Tlgx/NYdlTw48qIAEAhXXXWJLwfgwcPrq2//vr151Uf1K238847L4+fPn16bfHFF68Pbwu77eb5yCOPtOsVJPqcbgvk9emil5Do27rSFoTr45ZZZpnajTfeWB/36quv5uEnnXRS03Ji+phPNV3MP5YToseP6Ie6Grf66qvX2v5JqE979NFH18dFDyGxXa2OPfbYPL7qFST6uu7ovdErCACU5ZLmdEtxUmB8dQcNGpSft4XVegW40TrrrJOvpBhuu+22+tUQqzbNjaItc/QYMnDgwPoVFENUw2N5YY011siV40pUxaMKHGI5sbzWcauttlq79sxRja7aake76qgeV6LCHm27Q1Syo1ePRtdcc02+j+r4euut12474sqMcdt8881zU5M4STOq7q1iO2J7AIAyBGsAACjAyYsAAFCAYA0AAAUI1gAAUIBgDQAABQjWAABQgGANAAAFCNYAAFCAYA0AAAUs3NUrML/FlecAAOg+uuv1C3t8sA7d9cMBmJsoGti3AT1Rdy6KagoCAAAFCNYAAFCAYA0AAAUI1i3Gjx+fxo4d2zRsxowZ+f7iiy9ORx99dKfnNWrUqDwNwIdB7I8mTpxYf/7ss8/mWyi5v4r9aONyABYUC8TJi/PihRdeSAMGDGgatvHGG6dnnnkmrbjiiumxxx7r1HzioPLQQw+lYcOGzY/VBJhnrfuva665Jk2aNClNnTo1rbfeenkfNzdRVLj11lvbDT/llFPa7TPDWmut9f5XGKCbEaxbxIFjm222qT+PavVWW22Vll566U7P44477kj77bdfWnXVVdPo0aPbje/Tp0866qijOjwYAXxQImiPGTOmU6+NfWME8EZbb711mj17dn4c1e4ll1wyDR8+vPRqAnQbgnWLmTNnpo022qj+/PHHH08PPvhgPoDET6bPP/98mj59eh4XB6T4+bTRlClT0sEHH5z++te/tgvOMX0EbaEa+CDNmjUrV6dD/CoX+7MIybEvi/1SVK1jPzd58uR6EWHEiBFp33337fQyIqRvuOGG82X9AboLwTq9XWE+4ogj8uMIznHQiYNMHGwuv/zydPrpp+cqTFSvb7jhhnT44Ye3m0ccuH70ox/lqs2VV16Zm4LEQakK0HHwOuCAA+b6synA/HLffffl4Nu7d+98i1/NYl8V+7m4hWgXHcG4sbDQKvaNjaIQAcD/E6zbRLvCqN7EgWb99ddPJ554Yj6ALL/88mnatGmdOqEnDlYrr7xyeuCBB3LF58UXX0w77bRTOuGEE9Iaa6yRfvWrX6WzzjprnpqUAJRw/fXXp6FDh6a77rorP4990bHHHpsOO+ywTs+jagYSv8qtsMIKCgQAHdArSJsq7EZ1Og4+ISoxcTvyyCM7PY+oZFfzigp3hPJjjjkmPfHEEzmsC9VAV+jXr18aMmRI/Xk0cYv908477zzP84pf8G688caSqwfQY6hYzxFNNW655ZZ6dTpOPIz209H8o/r5s7WN9Tu1QYz5RKju27dvPhDFrVFH7bMB5ofW/VRUm6MJSDRZu+yyy/KwxjbWsa+7+eab669v7BEkCg4xPqaLabbccst09tlnf3AbA/AhJljPESf2xAEiRJgeNGhQfhztDaufQN+pjXWIttq/+93v0jnnnJP69+9fb7vYKtoydrbbPoD5JQJ3FbrfqY31yJEj869uIQoNUTSI10WzkKp5SZwUCbCg0xRkjuhvOtpIR6X57rvvTttuu+08TR8Hm2hTHaJdNUBP0dieurHnpCpUhwjfcT4JwIJMsJ6jaiMdog/qOPlwXlxwwQW5Qh3zeKeLLAB0V/GrXfXLXqsI384jARZ0gnWL6GLvoosuSj/+8Y/zT6Od5YAC9GTRrnrcuHHpwAMPzM3eImTfdtttuUckAN6mjfUc0Q/12LFjc3d7cVJhNA259957m/ptbT15McytX+r4ubS1z9dKVLb32Wef8hsB0EmtlyhvvUBMqM4vqfaPce5JNAOJosPJJ5+c95WNPYtUba5jPi4WAyyIBOv0dmCOg0ZjLx9xcGk8cXFeDR48eK7TzkslHGB+qE5G7Iy4+FXVx3+IJm9zO4k7Cg977bWX9tbAAqlXrU1Xr8T81KtXr9TDNxFYANm3AT1Vd96/aWMNAAAFCNYAAFCAYA0AAAUI1gAAUMAC0StINIIH6Gns2wA+XBaIYN1dzywFmJvufNY8wDvpzkUDTUEAAKAAwRoAAAoQrAEAoIAFoo31+xGXH3/hhReaLv8bl0CPS54DdBd33HFHOuKIIzr12lNOOSUNGDBgPq8RQM8jWM+x9dZbNz0fMWJE2nffffPj3r17N40bPXp02myzzdLhhx9eHzZjxoy08cYbd3p5ceBqnB5gfpo9e3a+nzp1an1YFA5C474o9oXVawGYN4L1HK0Hm6hSd2TixImpT58+7ULxRhtt5Ax94ENt5syZTUWEBx98MN9Pnz696TUAvDeC9Ty4+OKL089+9rOmEA7QXQwePLhTFWsA3hvBeo6oRF922WX5cVRx9tlnn3bjq1CtfTUAAK0E6zmi6ceYMWPSqFGjOhy/884755tQDXRXmoIAzF+C9RwRrFdcccWmYf37968/bjzwVKKtdTQPadSZqwU5cRH4oMV5IM8991zTsKOPPjqNHDlSDyAAhfSq9fAz7jp72d+o4hxzzDH54NOosQ3iUkst1e7ABNAVSlzSfG7ziK75hG2gq5TYv3UVFesG48aNqzf1aO1OL2y55ZZpypQpafjw4V2xegDvydixY9OsWbPy46r5R9++ffOt9WTFqp9+J2kDzDvBeo7Gg0g073jsscfavWaHHXZIV111lWANdCvHH398u/ND4te46KO/6q+/En3yn3DCCR/k6gH0GC5p3oGOQnUYNmxYmjZtWj7wAHQXraE6qtLnnHNOPiG7I/GLHQDzTrCei379+jU9jzaHEyZMSJMmTUrbbbddfg7Q3USTkGj+cfrpp3fYy9ENN9zQBWsF0DMI1g2iEh236AGksYeQyZMnp7322iufPR8nN5599tlp4MCB+afUqPzENNHQfl5vLsQAfFBiPxW9gMS5IqeeempTk7bqV7hq/7f22mt31WoCdGvaWDeIkxeff/75fOCpegeJA0x0xXfiiSfWXxd9XUfwvvvuu3PFx+XMgQ+z+IUt+umPC1/ddNNN7SrVF154Yf4lLq7MOGLECOeRALxHutsD6Ibs24Ceqjvv3zQFAQCAAgRrAAAoQLAGAIACBGsAAChggegVJBrBA/Q09m0AHy49Plh317NKAQDoXjQFAQCAAgRrAAAoQLAGAIACBGsAAChAsAYAgAIEawAAKECwBgCAAgRrAAAoQLAGAIACBGsAAChAsAYAgAIEawAAKECwBgCAAgRrAAAoQLAGAIACBGsAAChAsAYAgAIEawAAKECwBgCAAgRrAAAoQLAGAIACBGsAAChAsAYAgAIEawAAKECwBgCAAgRrAAAoQLAGAIACBGsAAChAsAYAgAIEawAAKECwBgCAAgRrAAAoQLAGAIACBGsAAChAsAYAgAIEawAAKECwBgCAAgRrAAAoQLAGAIACBGsAAChAsAYAgAIEawAAKECwBgCAAgRrAAAoQLAGAIACBGsAAChAsAYAgAIEawAAKECwBgCAAgRrAAAoQLAGAIACBGsAAChAsAYAgAIEawAAKECwBgCAAgRrAAAoQLAGAIACBGsAAChAsAYAgAIEawAAKECwBgCAAv4P7wF8EioVSTkAAAAASUVORK5CYII=)

pdf文件：

![](data:image/png;base64,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/Mgjj9SuscYatR06dKj94x//OKc8G5aV5csb7vxnBgwY0Gj9mcL6s9fQoUPnW14YDrJx88ML2/DWW2/l2pi1tWFZ1uZManl++fIBIV+e16tXrznLVhgeUqcHAIqHy5Zo8nr37h0HHHBArrPonXfeWa9s5MiRsf3228fdd98dHTt2jBdffDGWW265eOedd3Ll559/fu617LLL5j5n4zQ0ZMiQ3DhLLbVUzJ49u17ZrrvuGhtuuGHcfvvtc4ZlnU4blufnn/nss8+iTZs2ufdbb731nHGnTp0a++23X6y++uq5afr37x/Tpk2LPfbYY844WR3Dhg3Lvc+WIaX8ww8/zC1fXUjIdXbNl+fb16pVq7jiiivijDPOqLds48aNy7UzW2fzmh4AKC5u1UqTd9BBB8X1118/Z4e80BNPPBFPPfXUnB33fEjIyz5nr2xH+dFHH83twGc7yYV22mmn2HvvvePII4+cq/6JEyfGrFmz4tJLL41u3brlXs8//3y98pYtW+bqzerPXlm4aCyknH766fH000/n2pzZYIMNokOHDnOmm1f78t5+++1ceUOFwShrS6F11lmn3vopdNVVV+XuyjO/6QGA4uLMA01edoQ+VXaGoEuXLvWO1mcee+yxOPHEE3PvC2/zWjh+tvPemOzWpNntLM8666zc50MOOSRuueWW2HPPPXOfs9tbnnLKKfHPf/4z9/m2226Lvn371qsjuwVn9urTp8+cYSeffHLccccdc+ppuLxZmx588MG45JJLcp//8Y9/5M4ybLrppsnrY2HW3SOPPJJblvxyAgDFR3igaAwYMCB+//vfx8CBA+vdDSmT3UUoG5454YQTorKyMve+cPzGwsNll12W+7ewvuwsRBYU8jv92WVK2bMNGtafDyuZV155JXc3p5tuuqlee7O7IOWny5x55pm51+WXXx77779/LrRklx7l683OdCwK2V2h+vXrlws02b8AQHESHljiZTvu999/f5x77rm5I/mFO/rZJUeZrJ9B9spkZw6y25ZmstuonnTSSbn3X3zxRe7f7OFb2TgXX3xxvT4L87L++uvHXXfdFcsvv/xc9c9PNk42/8Jbu2ZhpfC2r9myjBo1Kvc+q3/HHXdcYL0LI1s/Wd+JY489NhccsjMoAEDxEh5Y4mX9BrIn7t56662xzz771CvLPy23sZ357Im722yzzZzP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)

### 笔记：

#### 图形图像简介及解析：

+ 大纲
  + 图形图像基本概念
  + Java图形图像关键类
  + 典型应用
    + 图像文件基本读写
    + 验证码生成
    + 统计图生成

+ 图形图像基础概念
  + 图形：Graph
    + 矢量图，根据几何特性来画的，比如点、直线、弧线等
  + 图像：Image
    + 由像素点组成
    + 格式：jpg，png，bmp，svg，wmf，gif，tiff等
    + 颜色：RGB(Red,Green, Blue)

+ Java图形图像关键类
  + 图形: Graph
    + java.awt包
    + Java 2D库：Graphics2D, Line2D, Rectangle2D, Ellipse2D,Arc2D
    + Color, Stroke
  + 图像: Image
    + javax.imageio包
    + ImageIO, BufferedImage, ImageReader, ImageWriter

+ Java图像关键类描述
  + Java原生支持jpg, png, bmp, wbmp, gif
  + javax.imageio.ImageIO
    + 自动封装多种ImageReader和ImageWriter，读写图像文
    + read读取图片 write 写图片
  + java.awt.image.BufferedImage,图像在内存中的表示类
    + getHeight获取高度
    + getWidth获取宽度
  + 图像文件读写/截取/合并

+ 验证码生成
  + 验证码，一个图片文件
    + 外框
    + 底色
    + 干扰线
      + 随机产生一些直线
    + 字母
      + 字母选择，不要0，o，1，I，L
      + 字母颜色(RGB)
      + 字母位置

+ 统计图生成
  + 统计图
    + 柱状图/饼图/折线图
    + Java原生的Graphics 2D可以画，比较繁琐
    + 基于jFreeChart(www.jfree.org/jfrecchart)可以快速实现统计图生成
      - 设定数据集
      - 调用ChartFactory生成图形

+ 总结
  - Java的AWT包提供了一些基础的图形工具Graphics 2D
  - Javax的imageio包提供了基础的图像读写和剪辑
  - 借助第三方库jFreeChart完成统计类图
  - API很多，需要多查询、多练习



#### 条形码和二维码简介及解析：

+ 条形码
  + 条形码(barcode)
    + 将宽度不等的多个黑条和空白，按照一定的编码规则排列，用以表达一组信息的图形标识符
    + 上个世纪40年代发明的
    + 通常代表一串数字/字母，每一位有特殊含义
    + 一般数据容量30个数字/字母
    + 专门机构管理：中国物品编码中心

+ 二维码
  + 用某种特定的几何图形按一定规律在平面（二维方向上）分布的黑白相间的图形记录数据符号信息
  + 比一维条形码能存更多信息，表示更多数据类型
  + 能够存储数字/字母/汉字/图片等信息
  + 字符集128个字符
  + 可存储几百到几十KB字符
  + 抗损坏

+ Barcode4J
  + http://barcode4j.sourceforge.net/
  + 纯Java实现的条形码生成
  + 只负责生成，不负责解析
  + 主要类
    + BarcodeUtil
    + BarcodeGencrator
    + DefaultConfiguration

+ 总结
  + 注意条形码种类
  + 高并发的时候，注意产生图片的速度
  + API很多，需要多查询、多练习



#### Docx简介及解析：

+ Docx简介
  + 以Microsoft Office的doc / docx为主要处理对象。
  + Word2003(包括)之前都是doc，文档格式不公开。
  + Word2007(包括)之后都是docx，遵循XML路线，文档格式公开。
  + docx为主要研究对象
    + 文字样式
    + 表格
    + 图片
    + 公式

+ Docx功能和处理
  + 常见功能
    - docx解析
    - docx生成（完全生成，模板加部分生成:套打)
  + 处理的第三方库
    - Jacob，COM4J (Windows平台)
    - POI，docx4j，OpenOfficc/Libre Office SDK(免费)
    - Aspose(收费)
    - 一些开源的OpenXML的包。

+ POI
  + Apache POI
  + Apache 出品，必属精品，poi.apache.org
  + 可处理docx，xlsx，pptx，visio等office套件
  + 纯Java工具包，无需第三方依赖
  + 主要类
    - XWPFDocument		整个文档对象
    - XWPFParagraph        段落
    - XWPFRun                    一个片段(字体样式相同的一段)
    - XWPFPicture               图片
    - XWPFTable                  表格

+ Doc/Docx处理总结
  + 不同的Office工具，产生出来的docx文件格式不兼容
  + 不同的第三方包，能够解析和生成的内容也不同。
  + doc/docx功能非常非常多，第三方包不是万能的。
  + API很多，需要多查询、多练习



#### 表格文件简介及解析：

+ 表格文件
  + xls/xlsx文件(Microsoft Excel)
  + CSV文件(Comma-Seperated Values文件)

+ xlsx(Excel)
  + 与word类似，也分成xls和xlsx。
  + xlsx以XML为标准，为主要研究对象
  + 数据
    - sheet
      - 行
      - 列
        - 单元格

+ xlsx(Excel)功能和第三方包
  + 常见功能
    + 解析
    + 生成
  + 第三方的包
    + POI，JXL（免费）
    + COM4J (Windows平台）
    + Aspose等（收费）

+ POI
  - Apache POI
    - Apache出品，必属精品，poi.apache.org
    - 可处理docx，xlsx，pptx，visio等office套件
    - 纯Java工具包，无需第三方依赖
    - 主要类
      - XSSFWorkbook   整个文档对象
      - XSSFSheet           单个sheet对象
      - XSSFRow              一行对象
      - XSSFCell               一个单元格对象

+ CSV文件
  + 全称:Comma-Seperated Values文件(逗号分隔)
  + 广义CSV文件，可以由空格/Tab键/分号/…/完成字段分隔
  + 第三方包：Apache Commons CSV
    + CSVFormat 文档格式
    + CSVParser解析文档
    + CSVRecord 一行记录
    + CSVPrinter 写入文档

+ 总结
  + 针对不同的表格文件格式，选择合适的第三方包
  + 大并发情况下，注意读写的速度
  + API很多，需要多查询、多练习



#### PDF简介及解析：

+ PDF
  - Portable Document Format的简称，意为“便携式文档格式”。
  - Adobe公司发明的。
  - PostScript，用以生成和输出图形，在任何打印机上都可保证精确的颜色和准确的打印效果。
  - 字型嵌入系统，可使字型随文件一起传输。
  - 结构化的存储系统，绑定元素和任何相关内容到单个文件，带有适当的数据压缩系统。
  - PDF的版本，一般是1.4+。

+ PDF处理和第三方包
  + 常见功能处理
    + 解析PDF
    + 生成PDF（转化）
  + 第三方包
    + Apache PDFBox（免费）
    + iText（收费）
    + XDocReport（将docx转化为pdf）

+ PDFBOx
  + Apache PDFBox
    + 纯]ava类库
    + 主要功能：创建，提取文本，分割/合并/删除，...
    + 主要类
      + PDDocument         pdf文档对象
      + PDFTextStripper    pdf文本对象
      + PDFMergerUtility   合并工具

+ XDocReport
  + 将docx文档合并输出为其他数据格式（pdf/htm/..）
  + PdfConverter
  + 基于poi和iText完成

+ pdf总结
  + pdf操作，可读取解析、合并、删除页面
  + 产生pdf和修改pdf，建议先生成docx，再进行转化
  + API很多，需要多查询、多练习