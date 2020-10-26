Dialog组件 代码：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dialog</title>
    <style>
        .container {
           max-width: 800px;
           margin: 30px auto;
           border-radius: 4px;
           box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.3);
           padding: 16px;
        }   
   
        .button {
           padding: 10px 16px;
           font-size: 14px;
           font-weight: 500;
           color: #303030;
           border: 1px solid #ccc;
           border-radius: 4px;
           outline: none;
           cursor: pointer;
           position: relative;
        }
   
        .button:hover {
           border-color: lightskyblue;
           color: lightskyblue;
        }

        .button.primary {
            background-color: lightskyblue;
            color: #fff;
            border: 0;
        }

        .dialog {
           position: fixed;
           top: 0;
           bottom: 0;
           left: 0;
           right: 0;
           background-color: rgba(0, 0, 0, 0.3);
           display: none;  
           opacity: 0;
           transition: all .3s;
        }

        .dialog.show {
            display: block;
        }

        .dialog.appear {    /*这里的小数点前面不能加空格！！！*/
           opacity: 1;
        }
        
        .dialog .main {
           width: 40%;
           background-color: #fff;
           margin: 30px auto;
           padding: 16px;
           border-radius: 6px;
           box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.3); 
           transform: translateY(-100%);
           opacity: 0;
           transition: all .3s;
        }

        .dialog.appear .main {
            transform: translateY(40px);
            opacity: 1;
        }

        .dialog .header {
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 20px;
        }

        .dialog .header .close {
            margin-left: auto;
            display: flex;
            justify-content: center;
            align-items: center;
            outline: none;
            border: none;
            cursor: pointer;
            padding: 4px;
        }

        .dialog .header .close::before,       /*两条线成十字形，然后分布旋转45度角 */
        .dialog .header .close::after {
            content: '';
            position: absolute;
            display: block;
            width: 10px;
            height: 1px;
            background: #333;
            transform: rotate(45deg);  /*旋转45度角 */
        }

        .dialog .header .close::after {
            transform: rotate(-45deg);
        }

        .dialog .footer {
            display: flex;
            justify-content: flex-end;  /*居于右下角 */
        }

        .dialog .footer .button {
            margin-left: 10px;
        }

    </style>
</head>
<body>
    <div class="container">
    <h2>Dialog</h2>
    <button class="button open-dialog">打开弹窗</button>
    <div class="dialog">
      <div class="main">
        <div class="header">提示 <button class="close"></button></div>
        <div class="content">
          <p>这是一段消息</p>
        </div>
        <div class="footer">
          <button class="button btn-cancel">取消</button>
          <button class="button primary btn-ok">确定</button>
        </div>
      </div>
    </div>
  </div>

    <script>
    class Dialog {
      constructor($root, options = {}) {
        this.$root = $root
        this.options = options
        this.onCancel = options.onCancel || function() {}
        this.onOk = options.onOk || function() {}

        this.bind()
      }

      bind() {
        let self = this
        this.$root.querySelector('.close').onclick = function() {
          self.hide()
          self.onCancel()
        }
        this.$root.querySelector('.btn-cancel').onclick = function() {
          self.hide()
          self.onCancel()
        }
        this.$root.querySelector('.btn-ok').onclick = function() {
          self.hide()
          self.onOk()
        }
      }

      hide() {
        this.$root.classList.remove('appear')
        setTimeout(() => this.$root.classList.remove('show'), 400)

      }

      show() {
        this.$root.classList.add('show')
        setTimeout(() => this.$root.classList.add('appear'))
      }
    }

    let dialog = new Dialog(document.querySelector('.dialog'), {
      onOk() {
        console.log('用户点了ok')
      },
      onCancel() {
        console.log('用户点了取消')
      }
    })

    document.querySelector('.open-dialog').onclick = function() {
      dialog.show()
    }

  </script>

</body>
</html>
