<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro de Ordem de Serviço</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background-color: #f4f4f9;
        }
        .container {
            width: 400px;
            padding: 20px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        h2 {
            text-align: center;
            color: #333;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            font-weight: bold;
            color: #555;
        }
        input, select {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 14px;
        }
        button {
            width: 100%;
            padding: 10px;
            border: none;
            background: #4CAF50;
            color: white;
            font-size: 16px;
            cursor: pointer;
            border-radius: 5px;
            margin-top: 10px;
        }
        button:hover {
            background: #45a049;
        }
        img {
            max-width: 100%;
            margin-top: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.4.0/jspdf.umd.min.js"></script>
</head>
<body>
    <div class="container">
        <h2>Registrar Ordem de Serviço____</h2>
        <form id="osForm">

            <div class="form-group">
                <label for="EquipeTécnica">Equipe Técnica:</label>
                <input type="text" id="EquipeTécnica" required>
            </div>
            <div class="form-group">
                <label for="edificio">Edifício:</label>
                <select id="edificio">
                    <option value="Sede">Sede</option>
                    <option value="Anexo A">Anexo A</option>
                    <option value="Anexo B">Anexo B</option>
                </select>
            </div>
            <div class="form-group">
                <label for="local">Local:</label>
                <input type="text" id="local" required>
            </div>
            <div class="form-group">
                <label for="equipamento">Equipamento:</label>
                <input type="text" id="equipamento" required>
            </div>
            <div class="form-group">
                <label for="data">Data:</label>
                <input type="date" id="data" required>
            </div>
            <div class="form-group">
                <label for="fotoAntes">Foto Antes:</label>
                <input type="file" id="fotoAntes" accept="image/*" required>
            </div>
            <div class="form-group">
                <label for="fotoDepois">Foto Depois:</label>
                <input type="file" id="fotoDepois" accept="image/*" required>
            </div>

            <div class="form-group">
                <label for="Observações">Procedimento de Execução:</label>
                <textarea id="Observações" placeholder="Detalhes do Serviço" rows="4"></textarea>
            </div>
            <div class="form-group">
                <label for="Materias">Materias:</label>
                <textarea id="Materias" placeholder="Ferramentas e Insumos utilizados" rows="4"></textarea>
            </div>
            <button type="button" id="gerarPDF">Gerar PDF</button>
        </form>
    </div>

    <script>
        document.getElementById('gerarPDF').addEventListener('click', async function() {
            const EquipeTécnica = document.getElementById('EquipeTécnica').value;
            const edificio = document.getElementById('edificio').value;
            const local = document.getElementById('local').value;
            const equipamento = document.getElementById('equipamento').value;
            const data = document.getElementById('data').value;
            const Observações = document.getElementById('Observações').value;
            const Materias = document.getElementById('Materias').value;

            const fotoAntesInput = document.getElementById('fotoAntes');
            const fotoDepoisInput = document.getElementById('fotoDepois');

            const fotoAntes = await readImageAsBase64(fotoAntesInput);
            const fotoDepois = await readImageAsBase64(fotoDepoisInput);

            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            doc.text("Ordem de Serviço", 10, 10);
            doc.text(Edifício: ${edificio}, 10, 20);
            doc.text(Local: ${local}, 90, 20);
            doc.text(Equipamento: ${equipamento}, 10, 30);
            doc.text(Data: ${data}, 10, 40);
            doc.text(Observações: ${Observações}, 10, 50);
            doc.text(Materias: ${Materias}, 90, 50);
            doc.text(EquipeTécnica: ${EquipeTécnica}, 10, 60);

            if (fotoAntes) {
                doc.text("Foto Antes:", 10, 130);
                doc.addImage(fotoAntes, 'JPEG', 10, 70, 50, 50);
            }

            if (fotoDepois) {
                doc.text("Foto Depois:", 80, 130);
                doc.addImage(fotoDepois, 'JPEG', 80, 70, 50, 50);
            }

            doc.save("Ordem_de_Servico.pdf");
        });

        function readImageAsBase64(inputElement) {
            return new Promise((resolve) => {
                const file = inputElement.files[0];
                if (!file) {
                    resolve(null);
                    return;
                }

                const reader = new FileReader();
                reader.onload = () => resolve(reader.result);
                reader.readAsDataURL(file);
            });
        }
    </script>
</body>
</html>
