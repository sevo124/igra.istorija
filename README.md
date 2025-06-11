# igra.istorija
<!DOCTYPE html>
<html lang="sr">
<head>
  <meta charset="UTF-8">
  <title>Poveži datume sa događajima</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: #f0f4f8;
      padding: 20px;
      color: #333;
    }
    h2 {
      text-align: center;
      color: #2e7d32;
    }
    .container {
      display: flex;
      flex-wrap: wrap;
      gap: 40px;
      justify-content: space-between;
    }
    .box {
      width: 40%;
    }
    .draggable {
      padding: 10px;
      margin: 10px 0;
      background-color: #e3f2fd;
      border: 1px solid #90caf9;
      border-radius: 5px;
      cursor: grab;
      font-size: 16px;
    }
    .droptarget {
      padding: 10px;
      margin: 10px 0;
      background-color: #e3f2fd;
      border: 2px dashed #aed581;
      min-height: 40px;
      border-radius: 5px;
      font-size: 16px;
    }
    .droptarget.filled {
      background-color: #e3f2fd;
      border-style: solid;
    }
    .buttons {
      text-align: center;
      margin-top: 30px;
    }
    button {
      background-color: #4caf50;
      color: white;
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      margin: 0 10px;
      cursor: pointer;
    }
    button:hover {
      background-color: #45a049;
    }
    .result {
      text-align: center;
      margin-top: 20px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h2>Poveži datume sa odgovarajućim događajima</h2>
  <div class="container">
    <div class="box" id="dates">
      <h3>Datumi</h3>
      <div class="draggable" draggable="true" id="d1">10.4.1941.</div>
      <div class="draggable" draggable="true" id="d2">25.5.1945.</div>
      <div class="draggable" draggable="true" id="d3">11-12.1943.</div>
      <div class="draggable" draggable="true" id="d4">1.11.1941.</div>
      <div class="draggable" draggable="true" id="d5">24.9.1941.</div>
      <div class="draggable" draggable="true" id="d6">4.7.1941.</div>
      <div class="draggable" draggable="true" id="d7">6.4.1941.</div>
      <div class="draggable" draggable="true" id="d8">5.1941.</div>
    </div>
    <div class="box" id="events">
      <h3>Događaji</h3>
      <div class="droptarget" data-answer="formiranje NDH">Formiranje NDH</div>
      <div class="droptarget" data-answer="kraj rata">Kraj rata</div>
      <div class="droptarget" data-answer="Teheranska konferencija">Teheranska konferencija</div>
      <div class="droptarget" data-answer="osnivanje NOVJ">Osnivanje NOVJ</div>
      <div class="droptarget" data-answer="osnivanje JV u O">Osnivanje JV u Otadžbini</div>
      <div class="droptarget" data-answer="formiranje uzicke republike">Formiranje Užičke republike</div>
      <div class="droptarget" data-answer="sukob cetnika i partizana">Sukob četnika i partizana</div>
      <div class="droptarget" data-answer="aprilski rat">Aprilski rat</div>
    </div>
  </div>

  <div class="buttons">
    <button onclick="checkAnswers()">Proveri</button>
    <button onclick="resetAll()">Resetuj</button>
  </div>
  <div class="result" id="result"></div>

  <script>
    const correctAnswers = {
      'd1': 'formiranje NDH',
      'd2': 'kraj rata',
      'd3': 'Teheranska konferencija',
      'd4': 'osnivanje NOVJ',
      'd5': 'formiranje uzicke republike',
      'd6': 'sukob cetnika i partizana',
      'd7': 'aprilski rat',
      'd8': 'osnivanje JV u O'
    };

    const draggables = document.querySelectorAll('.draggable');
    const droptargets = document.querySelectorAll('.droptarget');

    draggables.forEach(elem => {
      elem.addEventListener('dragstart', e => {
        e.dataTransfer.setData('text/plain', e.target.id);
      });
    });

    droptargets.forEach(target => {
      target.addEventListener('dragover', e => {
        e.preventDefault();
      });
      target.addEventListener('drop', e => {
        e.preventDefault();
        const draggedId = e.dataTransfer.getData('text');
        const draggedElem = document.getElementById(draggedId);

        if (target.querySelector('.draggable')) {
          target.removeChild(target.querySelector('.draggable'));
        }

        target.appendChild(draggedElem);
        target.classList.add('filled');
      });
    });

    function checkAnswers() {
      let score = 0;
      droptargets.forEach(target => {
        const answer = target.getAttribute('data-answer');
        const child = target.querySelector('.draggable');
        if (child && correctAnswers[child.id] === answer) {
          score++;
          target.style.borderColor = 'green';
        } else {
          target.style.borderColor = 'red';
        }
      });
      document.getElementById('result').textContent = `Tačno: ${score} od 8`;
    }

    function resetAll() {
      const dateBox = document.getElementById('dates');
      draggables.forEach(elem => {
        dateBox.appendChild(elem);
      });
      droptargets.forEach(target => {
        target.classList.remove('filled');
        target.style.borderColor = '#aed581';
      });
      document.getElementById('result').textContent = '';
    }
  </script>
</body>
</html>

