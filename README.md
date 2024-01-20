# laptrinhmangt3

Ho Ten: Nguyen Viet Hoang 
lop CNTT15-04
MSV 1571020108


package com.mycompany.dongboluong;

public class DongBoLuong {
    private static int sharedVariable = 0;
    private static Object lock = new Object();

    public static void main(String[] args) {
        // Tạo và khởi chạy luồng 1
        Thread thread1 = new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (lock) {
                    // Các công việc của luồng 1 ở đây
                    for (int i = 0; i < 5; i++) {
                        sharedVariable++;
                        System.out.println("Luong 1: " + sharedVariable);
                    }
                }
            }
        });

        // Tạo và khởi chạy luồng 2
        Thread thread2 = new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (lock) {
                    // Các công việc của luồng 2 ở đây
                    for (int i = 0; i < 5; i++) {
                        sharedVariable++;
                        System.out.println("Luong 2: " + sharedVariable);
                    }
                }
            }
        });

        // Tạo và khởi chạy tiến trình treo
        Thread threadTreo = new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    // Thực hiện một công việc nào đó trong tiến trình
                    for (int i = 0; i < 5; i++) {
                        System.out.println("Tien trinh dang chay...");
                        Thread.sleep(1000); // Treo tiến trình trong 1 giây
                    }
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        // Khởi chạy cả ba luồng/tiến trình
        thread1.start();
        thread2.start();
        threadTreo.start();

        // Đảm bảo rằng chương trình chính đợi cả ba luồng/tiến trình hoàn thành trước khi kết thúc
        try {
            thread1.join();
            thread2.join();
            threadTreo.join(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // Kiểm tra xem tiến trình treo còn đang chạy hay không
        if (threadTreo.isAlive()) {
            System.out.println("Tien trinh treo đang chay, se interrupt và ket thuc.");
            threadTreo.interrupt(); // Gián đoạn tiến trình treo nếu nó đang chạy
        } else {
            System.out.println("Tien trinh treo đa ket thuc.");
        }
    }
}
