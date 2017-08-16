# convolutional
#include<iostream>
#include<vector>
using namespace std;

vector<vector<int >>  conv(vector<vector<int >> &data,vector<vector<int >> &kernel , int type){
	vector<vector<int >> out;
	int h = data.size();
	int w = data[0].size();
	int k_h = kernel.size();
	int k_w = kernel[0].size();
	int hh,ww;//卷积之后的图片大小
	vector<vector<int >> aug;
	switch(type){
		case 0://full
			for(int i = 0; i < h + 2*k_h -2; i++){
				vector<int > t;
				for(int j = 0; j < w + 2*k_w -2; j++){
					if(i < k_h || i >= h + k_h
						|| j < k_w || j >= w + k_w)
						t.push_back(0);
					else{
						t.push_back(data[i-k_h][j-k_w]);
					}
				}
				aug.push_back(t);
			}
			hh = h + k_h -1;
			ww = w + k_w -1;
			break;
		case 1://same
			for(int i = 0; i < h + k_h -1; i++){
				vector<int > t;
				for(int j = 0; j < w + k_w -1; j++){
					if(i < k_h/2 || i >= h + k_h/2
						|| j < k_w/2 || j >= w + k_w/2)
						t.push_back(0);
					else{
						t.push_back(data[i-k_h/2][j-k_w/2]);
					}
				}
				aug.push_back(t);
			}
			hh = h;
			ww = w;
			break;
		case 2://valid
			for(int i = 0; i < h ; i++){
				vector<int > t;
				for(int j = 0; j < w ; j++){
					t.push_back(data[i][j]);
				}
				aug.push_back(t);
			}
			hh = h - k_h +1;
			ww = w - k_w +1;
			break;
		default:
			break;

	}
	

	for(int i = 0; i < hh; i++){
		vector<int > t;
		for(int j = 0; j < ww ; j++){
			int temp = 0;
			for(int r = 0 ; r < k_h; r++ ){
				for(int c = 0 ; c < k_w; c++ ){
					temp += kernel[r][c] * aug[i+r][j+c];
				}
			}
			t.push_back(temp);
		}
		out.push_back(t);
	}
	return out;
}
int main(){
	
	int a[5][5]={1,4,5,2,3,6,7,3,8,2,5,3,4,5,6,2,1,3,5,2,2,1,3,5,2};
	vector<vector<int >> v;
	for(int i = 0; i < 5 ; i++){
		vector<int > vv;
		for(int j = 0; j < 5 ; j++){
			vv.push_back(a[i][j]);
		}
		v.push_back(vv);
	}
	
	int b[3][3]={1,3,2,0,1,2,0,1,3};
	vector<vector<int >> k;
	for(int i = 0; i < 3 ; i++){
		vector<int > vv;
		for(int j = 0; j < 3 ; j++){
			vv.push_back(b[i][j]);
		}
		k.push_back(vv);
	}

	vector<vector<int >> out = conv(v,k,0);
	return 0;
}
